---
title: "Создание серверных служб для собственных мобильных приложений"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: 2edb704ea6875e8aa70e79fe085cc0edbe0c9a55
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="bc7f5-102">Создание серверных служб для собственных мобильных приложений</span><span class="sxs-lookup"><span data-stu-id="bc7f5-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="bc7f5-103">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bc7f5-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bc7f5-104">Мобильные приложения могут легко взаимодействовать с ASP.NET Core серверных служб.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="bc7f5-105">Просмотреть или загрузить образец кода службы серверной части</span><span class="sxs-lookup"><span data-stu-id="bc7f5-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="bc7f5-106">Пример собственного мобильного приложения</span><span class="sxs-lookup"><span data-stu-id="bc7f5-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="bc7f5-107">Демонстрирует создание серверных служб с помощью ASP.NET MVC основных компонентов для поддержки собственных мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="bc7f5-108">Она использует [приложения Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) как его собственного клиента, которая содержит отдельные собственных клиентов для устройств Android, iOS, универсальное приложение для Windows и Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="bc7f5-109">Можно следовать связанного учебника, чтобы создать собственное приложение (и установить необходимые бесплатные средства Xamarin), а также Загрузка примера решения Xamarin.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="bc7f5-110">Пример Xamarin включает проекте служб ASP.NET Web API 2 приложения ASP.NET Core в этой статье заменяет (нет изменений, требуемых клиентом).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Выполните остальные приложению, выполняющемуся на смартфоне Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="bc7f5-112">Функции</span><span class="sxs-lookup"><span data-stu-id="bc7f5-112">Features</span></span>

<span data-ttu-id="bc7f5-113">Приложение ToDoRest поддерживает перечисление, добавление, удаление и обновление элементов задач.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="bc7f5-114">Каждый элемент имеет идентификатор, имя, заметки и свойство, указывающее, было ли оно уже выполняется еще.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="bc7f5-115">Основного представления элементов, как показано выше, выводит список имен каждого элемента и указывает, если он выполняется с флажком.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-115">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="bc7f5-116">Коснувшись `+` значок открывает диалоговое окно добавления элемента:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-116">Tapping the `+` icon opens an add item dialog:</span></span>

![Диалоговое окно добавления элемента](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="bc7f5-118">При касании элемента на экране основного списка откроется диалоговое окно редактирования, где можно изменить имя элемента, заметки и Готово параметры, или элемент может быть удален:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Изменение элемента диалогового окна](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="bc7f5-120">В этом примере настраивается по умолчанию для использования серверных служб, размещенных на developer.xamarin.com, которые позволяет выполнять операции только для чтения.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="bc7f5-121">Можно протестировать их самостоятельно для приложения ASP.NET Core, созданные в следующем разделе, запущенными на локальном компьютере, необходимо обновить приложение `RestUrl` константой.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="bc7f5-122">Перейдите к `ToDoREST` проект и откройте *Constants.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="bc7f5-123">Замените `RestUrl` URL-адрес, включающий компьютера IP адрес (не "localhost" или "127.0.0.1, так как этот адрес используется в эмуляторе устройства не со своего компьютера).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="bc7f5-124">Включите имя порта (5000).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-124">Include the port number as well (5000).</span></span> <span data-ttu-id="bc7f5-125">Для тестирования работы служб с устройством, убедитесь, что у вас нет активных брандмауэр блокирует доступ к этому порту.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="bc7f5-126">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc7f5-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="bc7f5-127">Создайте новое веб-приложение ASP.NET Core в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="bc7f5-128">Выберите шаблон веб-API и без проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="bc7f5-129">Назовите проект *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-129">Name the project *ToDoApi*.</span></span>

![Диалоговое окно создания веб-приложения ASP.NET с выбранного шаблона проекта веб-API](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="bc7f5-131">Приложение должно отвечать на все запросы, адресованные port 5000.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="bc7f5-132">Обновление *Program.cs* для включения `.UseUrls("http://*:5000")` для этого:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="bc7f5-133">Убедитесь, что приложение запущено непосредственно, а не за IIS Express, которое не обрабатывает запросы нелокальных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="bc7f5-134">Запустите `dotnet run` из командной строки, или выберите имя профиля приложения в раскрывающемся списке целевой объект отладки в панели инструментов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-134">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="bc7f5-135">Добавьте класс модели для представления элементов задач.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="bc7f5-136">Пометить необходимые поля с помощью `[Required]` атрибута:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="bc7f5-137">Методы API требуется какой-либо способ работы с данными.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="bc7f5-138">Используйте тот же `IToDoRepository` интерфейс исходные образцы использования Xamarin:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="bc7f5-139">В этом образце реализация использует только частной коллекции элементов:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="bc7f5-140">Настройте реализацию в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="bc7f5-141">На этом этапе вы будете готовы создать *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="bc7f5-142">Узнайте больше о создании веб-API в [построение свой первый веб-API с Visual Studio и ASP.NET Core MVC](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="bc7f5-143">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="bc7f5-143">Creating the Controller</span></span>

<span data-ttu-id="bc7f5-144">Добавьте в проект новый контроллер *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="bc7f5-145">Он должен наследовать от Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="bc7f5-146">Добавить `Route` атрибут, чтобы указать, что контроллер будет обрабатывать запросы, адресованные пути, начинающиеся с `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="bc7f5-147">`[controller]` В маршруте заменяется имя контроллера (пропуск `Controller` суффикс) и особенно полезно для глобального маршрутов.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="bc7f5-148">Дополнительные сведения о [маршрутизации](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="bc7f5-149">Роли контроллера требуется `IToDoRepository` для функции; запроса экземпляра этого типа через конструктор контроллера.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="bc7f5-150">Во время выполнения, этот экземпляр будет предоставляться с использованием поддержки framework [внедрения зависимостей](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="bc7f5-151">Этот API поддерживает четыре различных HTTP-команд для выполнения операций CRUD (Создание, чтение, обновление, удаление) в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="bc7f5-152">Самый простой из них является операции чтения, которая соответствует запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="bc7f5-153">Чтение элементов</span><span class="sxs-lookup"><span data-stu-id="bc7f5-153">Reading Items</span></span>

<span data-ttu-id="bc7f5-154">Запрашивает список элементов, выполняется с помощью запроса GET к `List` метод.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="bc7f5-155">`[HttpGet]` Атрибут `List` метод указывает, что это действие только должна обрабатывать запросы GET.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="bc7f5-156">Маршрут для этого действия будет маршрута, указанного на контроллере.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="bc7f5-157">Не обязательно будет использовать имя действия в рамках маршрута.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="bc7f5-158">Необходимо убедиться, что каждое действие имеет уникальный и однозначный маршрута.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="bc7f5-159">Атрибуты маршрутизации может применяться в контроллер и уровни метод для создания определенных маршрутов.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="bc7f5-160">`List` Метод возвращает код ответа 200 ОК и все элементы ToDo, сериализованным в JSON.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="bc7f5-161">Можно проверить новый метод API с помощью различных средств, таких как [почтальон](https://www.getpostman.com/docs/), показано ниже:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Почтальон консоль, показывающая запрос GET для todoitems и текст ответа, показывающая JSON для трех элементов, возвращенных](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="bc7f5-163">Создание элементов</span><span class="sxs-lookup"><span data-stu-id="bc7f5-163">Creating Items</span></span>

<span data-ttu-id="bc7f5-164">По соглашению создание новых элементов данных сопоставляется с HTTP-команду POST.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="bc7f5-165">`Create` Имеет метод `[HttpPost]` атрибут, применяемый к нему и принимает `ToDoItem` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="bc7f5-166">Поскольку `item` аргумент будет передаваться в тексте запроса POST, этот параметр снабжен `[FromBody]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="bc7f5-167">Внутри метода элемент проверяется на допустимость и предыдущих существование в хранилище данных и при возникновении проблем нет, он добавляется с помощью репозитория.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="bc7f5-168">Проверка `ModelState.IsValid` выполняет [проверка модели](../mvc/models/validation.md)и необходимо сделать в каждый метод API, принимающее вводимые пользователем данные.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="bc7f5-169">Образец использует перечисление, содержащий коды ошибок, которые передаются клиенту мобильных устройств:</span><span class="sxs-lookup"><span data-stu-id="bc7f5-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="bc7f5-170">Проверить, добавление новых элементов, с помощью почтальон, выбрав команду POST, предоставляющего новый объект в формате JSON в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="bc7f5-171">Также следует добавить указания заголовка запроса `Content-Type` из `application/json`.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Почтальон консоль, показывающая POST и ответ](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="bc7f5-173">Метод возвращает вновь созданный элемент в ответе.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="bc7f5-174">Обновление элементов</span><span class="sxs-lookup"><span data-stu-id="bc7f5-174">Updating Items</span></span>

<span data-ttu-id="bc7f5-175">Изменение записей происходит с помощью HTTP-запросы PUT.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="bc7f5-176">За исключением этого изменения `Edit` метод практически идентичен `Create`.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="bc7f5-177">Обратите внимание, что, если запись не будет найден, `Edit` действие будет возвращать `NotFound` ответов (404).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="bc7f5-178">Для тестирования с почтальон сделать команды PUT.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="bc7f5-179">Обновленный объект данных указывается в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-179">Specify the updated object data in the Body of the request.</span></span>

![Почтальон консоль, показывающая PUT и ответ](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="bc7f5-181">Этот метод возвращает `NoContent` ответа (204) при успешном выполнении, чтобы обеспечить согласованность с существующие API.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="bc7f5-182">Удаление элементов</span><span class="sxs-lookup"><span data-stu-id="bc7f5-182">Deleting Items</span></span>

<span data-ttu-id="bc7f5-183">Удаление записей выполняется путем внесения запросов на удаление службы и передавая идентификатор элемента для удаления.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="bc7f5-184">Как при обновлении будет получать запросы для элементов, которые не существуют `NotFound` ответов.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="bc7f5-185">В противном случае будет получить успешный запрос `NoContent` ответа (204).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="bc7f5-186">Обратите внимание, что при тестировании функции удаления, никаких действий не требуется в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Почтальон консоль, показывающая DELETE и ответ](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="bc7f5-188">Общие соглашения веб-API</span><span class="sxs-lookup"><span data-stu-id="bc7f5-188">Common Web API Conventions</span></span>

<span data-ttu-id="bc7f5-189">При разработке серверных служб для вашего приложения, может потребоваться придумать согласованный набор соглашений или политики для обработки проблем с перекрестными.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="bc7f5-190">Например, в службе, показанном выше, запрашивает определенных записей, которые не были найдены получил `NotFound` ответа, а не `BadRequest` ответа.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="bc7f5-191">Аналогичным образом, команды, внесенные в эту службу, переданный типы привязано к модели, всегда проверяется `ModelState.IsValid` и возвращается `BadRequest` для типов Недопустимая модель.</span><span class="sxs-lookup"><span data-stu-id="bc7f5-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="bc7f5-192">После определения общую политику для собственные интерфейсы API можно обычно инкапсулировать его в [фильтра](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="bc7f5-193">Дополнительные сведения о [как инкапсулируют общую политики API в приложениях ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc7f5-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
