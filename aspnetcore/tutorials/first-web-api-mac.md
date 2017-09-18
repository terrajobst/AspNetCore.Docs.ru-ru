---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac"
description: "Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, веб-API, REST, mac, macOS, HTTP, служба, служба HTTP"
manager: wpickett
ms.openlocfilehash: 992059f7abd7650f82c1307acf3ba3219a6fcbb5
ms.sourcegitcommit: 0a3f215b4f665afc6f2678642968eea698102346
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="25d83-104">Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="25d83-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="25d83-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson) (Mike Wasson)</span><span class="sxs-lookup"><span data-stu-id="25d83-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="25d83-106">В этом учебнике вы создадите веб-API для управления списком дел.</span><span class="sxs-lookup"><span data-stu-id="25d83-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="25d83-107">Вы не будете создавать пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="25d83-107">You won’t build a UI.</span></span>

<span data-ttu-id="25d83-108">Существует 3 версии этого учебника.</span><span class="sxs-lookup"><span data-stu-id="25d83-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="25d83-109">macOS: создание веб-API с помощью Visual Studio для Mac (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="25d83-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="25d83-110">Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="25d83-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="25d83-111">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="25d83-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="25d83-112">Пример использования постоянной базы данных см. в статье [Введение в MVC ASP.NET Core на Mac или Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="25d83-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25d83-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="25d83-113">Prerequisites</span></span>

<span data-ttu-id="25d83-114">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="25d83-114">Install the following:</span></span>

- [<span data-ttu-id="25d83-115">Пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="25d83-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="25d83-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="25d83-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="25d83-117">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="25d83-117">Create the project</span></span>

<span data-ttu-id="25d83-118">В Visual Studio выберите **Файл > Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="25d83-118">From Visual Studio, select **File > New Solution**.</span></span>

![Новое решение macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="25d83-120">Выберите **Приложение .NET Core > Веб-API ASP.NET Core > Далее**.</span><span class="sxs-lookup"><span data-stu-id="25d83-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="25d83-122">Введите **TodoApi** в поле **Имя проекта** и выберите команду "Создать".</span><span class="sxs-lookup"><span data-stu-id="25d83-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="25d83-124">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="25d83-124">Launch the app</span></span>

<span data-ttu-id="25d83-125">В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="25d83-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="25d83-126">Visual Studio откроет браузер и перейдет к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="25d83-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="25d83-127">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="25d83-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="25d83-128">Измените URL-адрес на `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="25d83-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="25d83-129">Отобразятся данные `ValuesController`</span><span class="sxs-lookup"><span data-stu-id="25d83-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="25d83-130">Добавление поддержки для Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="25d83-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="25d83-131">Установите поставщик базы данных [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="25d83-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="25d83-132">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="25d83-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="25d83-133">В меню **Проект** выберите пункт **Добавить пакеты NuGet**.</span><span class="sxs-lookup"><span data-stu-id="25d83-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="25d83-134">Также можно щелкнуть **Зависимости** правой кнопкой мыши и выбрать команду **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="25d83-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="25d83-135">Введите `EntityFrameworkCore.InMemory` в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="25d83-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="25d83-136">Выберите `Microsoft.EntityFrameworkCore.InMemory`, а затем **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="25d83-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="25d83-137">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="25d83-137">Add a model class</span></span>

<span data-ttu-id="25d83-138">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="25d83-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="25d83-139">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="25d83-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="25d83-140">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="25d83-140">Add a folder named *Models*.</span></span> <span data-ttu-id="25d83-141">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="25d83-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="25d83-142">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="25d83-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="25d83-143">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="25d83-143">Name the folder *Models*.</span></span>

![Новая папка](first-web-api-mac/_static/folder.png)

<span data-ttu-id="25d83-145">Примечание. Классы моделей можно размещать в любом месте проекта, но обычно используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="25d83-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="25d83-146">Добавьте класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="25d83-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="25d83-147">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить > Новый файл > Общие > Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="25d83-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="25d83-148">Присвойте классу имя `TodoItem` и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="25d83-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="25d83-149">Замените сгенерированный код на следующий:</span><span class="sxs-lookup"><span data-stu-id="25d83-149">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="25d83-150">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="25d83-150">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="25d83-151">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="25d83-151">Create the database context</span></span>

<span data-ttu-id="25d83-152">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="25d83-152">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="25d83-153">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="25d83-153">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="25d83-154">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="25d83-154">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="25d83-155">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="25d83-155">Add a controller</span></span>

<span data-ttu-id="25d83-156">В обозревателе решений добавьте в папку *Controllers* класс `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="25d83-156">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="25d83-157">Замените сгенерированный код на следующий (и добавьте закрывающие скобки).</span><span class="sxs-lookup"><span data-stu-id="25d83-157">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="25d83-158">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="25d83-158">Launch the app</span></span>

<span data-ttu-id="25d83-159">В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="25d83-159">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="25d83-160">Visual Studio запустит браузер и перейдет к `http://localhost:port`, где *порт* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="25d83-160">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="25d83-161">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="25d83-161">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="25d83-162">Измените URL-адрес на `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="25d83-162">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="25d83-163">Отобразятся данные `ValuesController`</span><span class="sxs-lookup"><span data-stu-id="25d83-163">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="25d83-164">Перейдите к контроллеру `Todo` по адресу `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="25d83-164">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="25d83-165">Реализация других операций CRUD</span><span class="sxs-lookup"><span data-stu-id="25d83-165">Implement the other CRUD operations</span></span>

<span data-ttu-id="25d83-166">Добавим к контроллеру методы `Create`, `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="25d83-166">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="25d83-167">Это вариации темы, поэтому мы просто покажем код и выделим основные различия.</span><span class="sxs-lookup"><span data-stu-id="25d83-167">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="25d83-168">После добавления или изменения кода выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="25d83-168">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="25d83-169">Создать</span><span class="sxs-lookup"><span data-stu-id="25d83-169">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="25d83-170">Это метод HTTP POST, обозначенный атрибутом [`[HttpPost]`](https://docs.microsoft.com/aspnet/core/api).</span><span class="sxs-lookup"><span data-stu-id="25d83-170">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.microsoft.com/aspnet/core/api) attribute.</span></span> <span data-ttu-id="25d83-171">Атрибут [`[FromBody]`](https://docs.microsoft.com/aspnet/core/api) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="25d83-171">The [`[FromBody]`](https://docs.microsoft.com/aspnet/core/api) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="25d83-172">Метод `CreatedAtRoute` возвращает ответ 201, который представляет собой стандартный ответ для метода HTTP POST, создающий новый ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="25d83-172">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="25d83-173">`CreatedAtRoute` также добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="25d83-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="25d83-174">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="25d83-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="25d83-175">См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="25d83-175">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="25d83-176">Отправка запроса Create с помощью коллекции Postman</span><span class="sxs-lookup"><span data-stu-id="25d83-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="25d83-177">Запустите приложение (**Выполнить > Запуск с отладкой**).</span><span class="sxs-lookup"><span data-stu-id="25d83-177">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="25d83-178">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="25d83-178">Start Postman.</span></span>

![Консоль Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="25d83-180">Укажите HTTP-метод `POST`</span><span class="sxs-lookup"><span data-stu-id="25d83-180">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="25d83-181">Установите переключатель **Текст запроса**</span><span class="sxs-lookup"><span data-stu-id="25d83-181">Select the **Body** radio button</span></span>
* <span data-ttu-id="25d83-182">Установите переключатель **без обработки**</span><span class="sxs-lookup"><span data-stu-id="25d83-182">Select the **raw** radio button</span></span>
* <span data-ttu-id="25d83-183">В качестве типа выберите JSON</span><span class="sxs-lookup"><span data-stu-id="25d83-183">Set the type to JSON</span></span>
* <span data-ttu-id="25d83-184">В редакторе пар ключей и значений введите элемент Todo, например</span><span class="sxs-lookup"><span data-stu-id="25d83-184">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="25d83-185">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="25d83-185">Select **Send**</span></span>

* <span data-ttu-id="25d83-186">Откройте вкладку "Заголовки" в нижней области и скопируйте заголовок **расположения**:</span><span class="sxs-lookup"><span data-stu-id="25d83-186">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="25d83-188">URI заголовка расположения позволяет получить доступ к только что созданному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="25d83-188">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="25d83-189">Снова вызовите метод `GetById` для создания маршрута с именем `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="25d83-189">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="25d83-190">Обновление</span><span class="sxs-lookup"><span data-stu-id="25d83-190">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="25d83-191">Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="25d83-191">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="25d83-192">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="25d83-192">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="25d83-193">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней.</span><span class="sxs-lookup"><span data-stu-id="25d83-193">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="25d83-194">Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="25d83-194">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="25d83-196">Удаление</span><span class="sxs-lookup"><span data-stu-id="25d83-196">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="25d83-197">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="25d83-197">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="25d83-199">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="25d83-199">Next steps</span></span>

* [<span data-ttu-id="25d83-200">Маршрутизация к действиям контроллера</span><span class="sxs-lookup"><span data-stu-id="25d83-200">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="25d83-201">Сведения о развертывании API см. в статье [Публикация и развертывание](../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="25d83-201">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="25d83-202">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="25d83-202">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="25d83-203">Postman</span><span class="sxs-lookup"><span data-stu-id="25d83-203">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="25d83-204">Fiddler</span><span class="sxs-lookup"><span data-stu-id="25d83-204">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
