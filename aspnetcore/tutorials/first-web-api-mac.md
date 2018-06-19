---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac
author: rick-anderson
description: Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
ms.locfileid: "33923208"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="6f45b-103">Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6f45b-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="6f45b-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="6f45b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="6f45b-105">В этом руководстве создается веб-API для управления элементами списка дел.</span><span class="sxs-lookup"><span data-stu-id="6f45b-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="6f45b-106">При этом пользовательский интерфейс не создается.</span><span class="sxs-lookup"><span data-stu-id="6f45b-106">The UI isn't constructed.</span></span>

<span data-ttu-id="6f45b-107">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="6f45b-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="6f45b-108">macOS: создание веб-API с помощью Visual Studio для Mac (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="6f45b-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="6f45b-109">Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="6f45b-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="6f45b-110">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="6f45b-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="6f45b-111">Пример использования постоянной базы данных см. в статье [Введение в MVC ASP.NET Core в macOS](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="6f45b-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f45b-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6f45b-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="6f45b-113">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="6f45b-113">Create the project</span></span>

<span data-ttu-id="6f45b-114">В Visual Studio выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![Новое решение macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="6f45b-116">Выберите **Приложение .NET Core** > **Веб-API ASP.NET Core** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="6f45b-118">Введите *TodoApi* в поле **Имя проекта** и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="6f45b-120">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6f45b-120">Launch the app</span></span>

<span data-ttu-id="6f45b-121">В Visual Studio откройте меню **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="6f45b-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="6f45b-122">Visual Studio откроет браузер и перейдет к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="6f45b-123">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="6f45b-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="6f45b-124">Измените URL-адрес на `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="6f45b-125">Отображаются данные `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="6f45b-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="6f45b-126">Добавление поддержки для Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6f45b-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="6f45b-127">Установите поставщик базы данных [Entity Framework Core InMemory](/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="6f45b-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="6f45b-128">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="6f45b-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="6f45b-129">В меню **Проект** выберите пункт **Добавить пакеты NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="6f45b-130">Также можно щелкнуть **Зависимости** правой кнопкой мыши и выбрать команду **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="6f45b-131">Введите `EntityFrameworkCore.InMemory` в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="6f45b-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="6f45b-132">Выберите `Microsoft.EntityFrameworkCore.InMemory`, а затем **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="6f45b-133">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="6f45b-133">Add a model class</span></span>

<span data-ttu-id="6f45b-134">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="6f45b-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="6f45b-135">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="6f45b-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="6f45b-136">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="6f45b-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="6f45b-137">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6f45b-138">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f45b-138">Name the folder *Models*.</span></span>

![Новая папка](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="6f45b-140">Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f45b-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="6f45b-141">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="6f45b-142">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="6f45b-143">Замените сгенерированный код на следующий:</span><span class="sxs-lookup"><span data-stu-id="6f45b-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6f45b-144">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="6f45b-145">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="6f45b-145">Create the database context</span></span>

<span data-ttu-id="6f45b-146">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="6f45b-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="6f45b-147">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="6f45b-148">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f45b-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="6f45b-149">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="6f45b-149">Add a controller</span></span>

<span data-ttu-id="6f45b-150">В обозревателе решений добавьте в папку *Controllers* класс `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="6f45b-151">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6f45b-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="6f45b-152">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6f45b-152">Launch the app</span></span>

<span data-ttu-id="6f45b-153">В Visual Studio откройте меню **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="6f45b-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="6f45b-154">Visual Studio запустит браузер и перейдет к `http://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="6f45b-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6f45b-155">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="6f45b-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="6f45b-156">Измените URL-адрес на `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="6f45b-157">Отображаются данные `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="6f45b-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="6f45b-158">Перейдите к контроллеру `Todo` по адресу `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="6f45b-159">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="6f45b-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="6f45b-160">Реализация других операций CRUD</span><span class="sxs-lookup"><span data-stu-id="6f45b-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="6f45b-161">Добавим к контроллеру методы `Create`, `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="6f45b-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="6f45b-162">Эти методы являются вариациями темы, поэтому мы просто покажем код и выделим основные различия.</span><span class="sxs-lookup"><span data-stu-id="6f45b-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="6f45b-163">После добавления или изменения кода выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="6f45b-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="6f45b-164">Создать</span><span class="sxs-lookup"><span data-stu-id="6f45b-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="6f45b-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="6f45b-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="6f45b-166">Приведенный выше метод отвечает на запрос HTTP POST, как указано атрибутом [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="6f45b-166">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6f45b-167">Атрибут [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="6f45b-167">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6f45b-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="6f45b-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="6f45b-169">Приведенный выше метод отвечает на запрос HTTP POST, как указано атрибутом [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="6f45b-169">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6f45b-170">MVC получает значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="6f45b-170">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="6f45b-171">Метод `CreatedAtRoute` возвращает ответ 201.</span><span class="sxs-lookup"><span data-stu-id="6f45b-171">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="6f45b-172">Это стандартный ответ для метода HTTP POST, который создает ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="6f45b-172">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="6f45b-173">`CreatedAtRoute` также добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="6f45b-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="6f45b-174">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="6f45b-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="6f45b-175">См. раздел [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6f45b-175">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="6f45b-176">Отправка запроса Create с помощью Postman</span><span class="sxs-lookup"><span data-stu-id="6f45b-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="6f45b-177">Запустите приложение (**Выполнить** > **Запуск с отладкой**).</span><span class="sxs-lookup"><span data-stu-id="6f45b-177">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="6f45b-178">Откройте Postman.</span><span class="sxs-lookup"><span data-stu-id="6f45b-178">Open Postman.</span></span>

![Консоль Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="6f45b-180">Измените номера порта в localhost URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="6f45b-180">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="6f45b-181">Укажите метод HTTP *POST*.</span><span class="sxs-lookup"><span data-stu-id="6f45b-181">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="6f45b-182">Откройте вкладку **Текст**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-182">Click the **Body** tab.</span></span>
* <span data-ttu-id="6f45b-183">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-183">Select the **raw** radio button.</span></span>
* <span data-ttu-id="6f45b-184">Задайте тип *JSON (приложение/json)*.</span><span class="sxs-lookup"><span data-stu-id="6f45b-184">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="6f45b-185">Введите текст запроса с элементом задачи наподобие следующего JSON:</span><span class="sxs-lookup"><span data-stu-id="6f45b-185">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="6f45b-186">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-186">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="6f45b-187">Если ответ не отображается после нажатия кнопки **Отправить**, отключите параметр **Проверка сертификации SSL**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-187">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="6f45b-188">Он находится в разделе **Файл** > **Параметры**.</span><span class="sxs-lookup"><span data-stu-id="6f45b-188">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="6f45b-189">Нажмите кнопку **Отправить** еще раз после отключения параметра.</span><span class="sxs-lookup"><span data-stu-id="6f45b-189">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="6f45b-190">Откройте вкладку **Заголовки** на панели **Ответ** и скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="6f45b-190">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="6f45b-192">URI заголовка расположения позволяет получить доступ к созданному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="6f45b-192">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="6f45b-193">Метод `Create` возвращает [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="6f45b-193">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="6f45b-194">Первый параметр, передаваемый `CreatedAtRoute`, представляет именованный маршрут для создания URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="6f45b-194">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="6f45b-195">Помните, что метод `GetById` создал именованный маршрут `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="6f45b-195">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="6f45b-196">Обновление</span><span class="sxs-lookup"><span data-stu-id="6f45b-196">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="6f45b-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="6f45b-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6f45b-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="6f45b-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="6f45b-199">Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6f45b-199">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="6f45b-200">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6f45b-200">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6f45b-201">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней.</span><span class="sxs-lookup"><span data-stu-id="6f45b-201">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="6f45b-202">Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="6f45b-202">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="6f45b-204">Удаление</span><span class="sxs-lookup"><span data-stu-id="6f45b-204">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="6f45b-205">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6f45b-205">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
