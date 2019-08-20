---
title: Учебник. Вызов веб-API ASP.NET Core с помощью jQuery
author: rick-anderson
description: Узнайте, как вызывать веб-API ASP.NET Core, используя jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: a319e4b4ce09e9b09afeaff065d5740276deb115
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022562"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a><span data-ttu-id="0c845-103">Учебник. Вызов веб-API ASP.NET Core с помощью jQuery</span><span class="sxs-lookup"><span data-stu-id="0c845-103">Tutorial: Call an ASP.NET Core web API with jQuery</span></span>

<span data-ttu-id="0c845-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="0c845-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0c845-105">Этот учебник описывает вызов веб-API ASP.NET Core с использованием jQuery.</span><span class="sxs-lookup"><span data-stu-id="0c845-105">This tutorial shows how to call an ASP.NET Core web API with jQuery</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c845-106">При использовании ASP.NET Core 2.2 см. учебник [Вызов веб-API с помощью jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery) для версии 2.2.</span><span class="sxs-lookup"><span data-stu-id="0c845-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the Web API with jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="0c845-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0c845-107">Prerequisites</span></span>

* <span data-ttu-id="0c845-108">Изучите [Учебник. Создание веб-API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="0c845-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="0c845-109">Знакомство с CSS, HTML, JavaScript и jQuery</span><span class="sxs-lookup"><span data-stu-id="0c845-109">Familiarity with CSS, HTML, JavaScript, and jQuery</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="0c845-110">Вызов API с помощью jQuery</span><span class="sxs-lookup"><span data-stu-id="0c845-110">Call the API with jQuery</span></span>

<span data-ttu-id="0c845-111">В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="0c845-111">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="0c845-112">jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="0c845-112">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="0c845-113">Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="0c845-113">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

<span data-ttu-id="0c845-114">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="0c845-114">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="0c845-115">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0c845-115">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="0c845-116">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="0c845-116">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="0c845-117">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0c845-117">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="0c845-118">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0c845-118">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="0c845-119">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="0c845-119">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="0c845-120">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0c845-120">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="0c845-121">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0c845-121">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="0c845-122">Для получения jQuery можно использовать следующие способы.</span><span class="sxs-lookup"><span data-stu-id="0c845-122">There are several ways to get jQuery.</span></span> <span data-ttu-id="0c845-123">В предыдущем фрагменте кода библиотека загружается из CDN.</span><span class="sxs-lookup"><span data-stu-id="0c845-123">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="0c845-124">В этом примере вызываются все методы CRUD в API.</span><span class="sxs-lookup"><span data-stu-id="0c845-124">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="0c845-125">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="0c845-125">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="0c845-126">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="0c845-126">Get a list of to-do items</span></span>

<span data-ttu-id="0c845-127">Функция jQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `GET` к API, который возвращает JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="0c845-127">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="0c845-128">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="0c845-128">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="0c845-129">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="0c845-129">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="0c845-130">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="0c845-130">Add a to-do item</span></span>

<span data-ttu-id="0c845-131">Функция [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `POST` с элементом списка дел в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="0c845-131">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="0c845-132">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="0c845-132">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="0c845-133">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="0c845-133">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="0c845-134">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="0c845-134">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="0c845-135">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="0c845-135">Update a to-do item</span></span>

<span data-ttu-id="0c845-136">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="0c845-136">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="0c845-137">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="0c845-137">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="0c845-138">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="0c845-138">Delete a to-do item</span></span>

<span data-ttu-id="0c845-139">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="0c845-139">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

<span data-ttu-id="0c845-140">Перейдите к следующему учебнику, посвященному созданию страниц справки по API:</span><span class="sxs-lookup"><span data-stu-id="0c845-140">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end
