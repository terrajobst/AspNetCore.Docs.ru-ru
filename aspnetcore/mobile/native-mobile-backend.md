---
title: "Создание серверных служб для собственных мобильных приложений"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: f2b74d6739112909ee05e036e904d5e30868fdbe
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="457ed-103">Создание серверных служб для собственных мобильных приложений</span><span class="sxs-lookup"><span data-stu-id="457ed-103">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="457ed-104">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="457ed-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="457ed-105">Мобильные приложения могут легко взаимодействовать с ASP.NET Core серверных служб.</span><span class="sxs-lookup"><span data-stu-id="457ed-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="457ed-106">Просмотреть или загрузить образец кода службы серверной части</span><span class="sxs-lookup"><span data-stu-id="457ed-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="457ed-107">Пример собственного мобильного приложения</span><span class="sxs-lookup"><span data-stu-id="457ed-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="457ed-108">Демонстрирует создание серверных служб с помощью ASP.NET MVC основных компонентов для поддержки собственных мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="457ed-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="457ed-109">Она использует [приложения Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) как его собственного клиента, которая содержит отдельные собственных клиентов для устройств Android, iOS, универсальное приложение для Windows и Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="457ed-109">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="457ed-110">Можно следовать связанного учебника, чтобы создать собственное приложение (и установить необходимые бесплатные средства Xamarin), а также Загрузка примера решения Xamarin.</span><span class="sxs-lookup"><span data-stu-id="457ed-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="457ed-111">Пример Xamarin включает проекте служб ASP.NET Web API 2 приложения ASP.NET Core в этой статье заменяет (нет изменений, требуемых клиентом).</span><span class="sxs-lookup"><span data-stu-id="457ed-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Выполните остальные приложению, выполняющемуся на смартфоне Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="457ed-113">Функции</span><span class="sxs-lookup"><span data-stu-id="457ed-113">Features</span></span>

<span data-ttu-id="457ed-114">Приложение ToDoRest поддерживает перечисление, добавление, удаление и обновление элементов задач.</span><span class="sxs-lookup"><span data-stu-id="457ed-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="457ed-115">Каждый элемент имеет идентификатор, имя, заметки и свойство, указывающее, было ли оно уже выполняется еще.</span><span class="sxs-lookup"><span data-stu-id="457ed-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="457ed-116">Основного представления элементов, как показано выше, выводит список имен каждого элемента и указывает, если он выполняется с флажком.</span><span class="sxs-lookup"><span data-stu-id="457ed-116">The main view of the items, as shown above, lists each item's name and indicates if it is done with a checkmark.</span></span>

<span data-ttu-id="457ed-117">Коснувшись `+` значок открывает диалоговое окно добавления элемента:</span><span class="sxs-lookup"><span data-stu-id="457ed-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Диалоговое окно добавления элемента](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="457ed-119">При касании элемента на экране основного списка откроется диалоговое окно редактирования, где можно изменить имя элемента, заметки и Готово параметры, или элемент может быть удален:</span><span class="sxs-lookup"><span data-stu-id="457ed-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Изменение элемента диалогового окна](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="457ed-121">В этом примере настраивается по умолчанию для использования серверных служб, размещенных на developer.xamarin.com, которые позволяет выполнять операции только для чтения.</span><span class="sxs-lookup"><span data-stu-id="457ed-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="457ed-122">Можно протестировать их самостоятельно для приложения ASP.NET Core, созданные в следующем разделе, запущенными на локальном компьютере, необходимо обновить приложение `RestUrl` константой.</span><span class="sxs-lookup"><span data-stu-id="457ed-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="457ed-123">Перейдите к `ToDoREST` проект и откройте *Constants.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="457ed-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="457ed-124">Замените `RestUrl` URL-адрес, включающий компьютера IP адрес (не "localhost" или "127.0.0.1, так как этот адрес используется в эмуляторе устройства не со своего компьютера).</span><span class="sxs-lookup"><span data-stu-id="457ed-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="457ed-125">Включите имя порта (5000).</span><span class="sxs-lookup"><span data-stu-id="457ed-125">Include the port number as well (5000).</span></span> <span data-ttu-id="457ed-126">Для тестирования работы служб с устройством, убедитесь, что у вас нет активных брандмауэр блокирует доступ к этому порту.</span><span class="sxs-lookup"><span data-stu-id="457ed-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "csharp"} -->

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="457ed-127">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="457ed-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="457ed-128">Создайте новое веб-приложение ASP.NET Core в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="457ed-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="457ed-129">Выберите шаблон веб-API и без проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="457ed-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="457ed-130">Назовите проект *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="457ed-130">Name the project *ToDoApi*.</span></span>

![Диалоговое окно создания веб-приложения ASP.NET с выбранного шаблона проекта веб-API](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="457ed-132">Приложение должно отвечать на все запросы, адресованные port 5000.</span><span class="sxs-lookup"><span data-stu-id="457ed-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="457ed-133">Обновление *Program.cs* для включения `.UseUrls("http://*:5000")` для этого:</span><span class="sxs-lookup"><span data-stu-id="457ed-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

<span data-ttu-id="457ed-134">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="457ed-134">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]</span></span>

> [!NOTE]
> <span data-ttu-id="457ed-135">Убедитесь, что приложение запущено непосредственно, а не за IIS Express, которое не обрабатывает запросы нелокальных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="457ed-135">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="457ed-136">Запустите `dotnet run` из командной строки, или выберите имя профиля приложения в раскрывающемся списке целевой объект отладки в панели инструментов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="457ed-136">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="457ed-137">Добавьте класс модели для представления элементов задач.</span><span class="sxs-lookup"><span data-stu-id="457ed-137">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="457ed-138">Пометить необходимые поля с помощью `[Required]` атрибута:</span><span class="sxs-lookup"><span data-stu-id="457ed-138">Mark required fields using the `[Required]` attribute:</span></span>

<span data-ttu-id="457ed-139">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="457ed-139">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]</span></span>

<span data-ttu-id="457ed-140">Методы API требуется какой-либо способ работы с данными.</span><span class="sxs-lookup"><span data-stu-id="457ed-140">The API methods require some way to work with data.</span></span> <span data-ttu-id="457ed-141">Используйте тот же `IToDoRepository` интерфейс исходные образцы использования Xamarin:</span><span class="sxs-lookup"><span data-stu-id="457ed-141">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

<span data-ttu-id="457ed-142">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]</span><span class="sxs-lookup"><span data-stu-id="457ed-142">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]</span></span>

<span data-ttu-id="457ed-143">В этом образце реализация использует только частной коллекции элементов:</span><span class="sxs-lookup"><span data-stu-id="457ed-143">For this sample, the implementation just uses a private collection of items:</span></span>

<span data-ttu-id="457ed-144">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]</span><span class="sxs-lookup"><span data-stu-id="457ed-144">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]</span></span>

<span data-ttu-id="457ed-145">Настройте реализацию в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="457ed-145">Configure the implementation in *Startup.cs*:</span></span>

<span data-ttu-id="457ed-146">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]</span><span class="sxs-lookup"><span data-stu-id="457ed-146">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]</span></span>

<span data-ttu-id="457ed-147">На этом этапе вы будете готовы создать *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="457ed-147">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="457ed-148">Узнайте больше о создании веб-API в [построение свой первый веб-API с Visual Studio и ASP.NET Core MVC](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="457ed-148">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="457ed-149">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="457ed-149">Creating the Controller</span></span>

<span data-ttu-id="457ed-150">Добавьте в проект новый контроллер *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="457ed-150">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="457ed-151">Он должен наследовать от Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="457ed-151">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="457ed-152">Добавить `Route` атрибут, чтобы указать, что контроллер будет обрабатывать запросы, адресованные пути, начинающиеся с `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="457ed-152">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="457ed-153">`[controller]` В маршруте заменяется имя контроллера (пропуск `Controller` суффикс) и особенно полезно для глобального маршрутов.</span><span class="sxs-lookup"><span data-stu-id="457ed-153">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="457ed-154">Дополнительные сведения о [маршрутизации](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="457ed-154">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="457ed-155">Роли контроллера требуется `IToDoRepository` для функции; запроса экземпляра этого типа через конструктор контроллера.</span><span class="sxs-lookup"><span data-stu-id="457ed-155">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="457ed-156">Во время выполнения, этот экземпляр будет предоставляться с использованием поддержки framework [внедрения зависимостей](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="457ed-156">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

<span data-ttu-id="457ed-157">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]</span><span class="sxs-lookup"><span data-stu-id="457ed-157">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]</span></span>

<span data-ttu-id="457ed-158">Этот API поддерживает четыре различных HTTP-команд для выполнения операций CRUD (Создание, чтение, обновление, удаление) в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="457ed-158">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="457ed-159">Самый простой из них является операции чтения, которая соответствует запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="457ed-159">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="457ed-160">Чтение элементов</span><span class="sxs-lookup"><span data-stu-id="457ed-160">Reading Items</span></span>

<span data-ttu-id="457ed-161">Запрашивает список элементов, выполняется с помощью запроса GET к `List` метод.</span><span class="sxs-lookup"><span data-stu-id="457ed-161">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="457ed-162">`[HttpGet]` Атрибут `List` метод указывает, что это действие только должна обрабатывать запросы GET.</span><span class="sxs-lookup"><span data-stu-id="457ed-162">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="457ed-163">Маршрут для этого действия будет маршрута, указанного на контроллере.</span><span class="sxs-lookup"><span data-stu-id="457ed-163">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="457ed-164">Не обязательно будет использовать имя действия в рамках маршрута.</span><span class="sxs-lookup"><span data-stu-id="457ed-164">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="457ed-165">Необходимо убедиться, что каждое действие имеет уникальный и однозначный маршрута.</span><span class="sxs-lookup"><span data-stu-id="457ed-165">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="457ed-166">Атрибуты маршрутизации может применяться в контроллер и уровни метод для создания определенных маршрутов.</span><span class="sxs-lookup"><span data-stu-id="457ed-166">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

<span data-ttu-id="457ed-167">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]</span><span class="sxs-lookup"><span data-stu-id="457ed-167">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]</span></span>

<span data-ttu-id="457ed-168">`List` Метод возвращает код ответа 200 ОК и все элементы ToDo, сериализованным в JSON.</span><span class="sxs-lookup"><span data-stu-id="457ed-168">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="457ed-169">Можно проверить новый метод API с помощью различных средств, таких как [почтальон](https://www.getpostman.com/docs/), показано ниже:</span><span class="sxs-lookup"><span data-stu-id="457ed-169">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Почтальон консоль, показывающая запрос GET для todoitems и текст ответа, показывающая JSON для трех элементов, возвращенных](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="457ed-171">Создание элементов</span><span class="sxs-lookup"><span data-stu-id="457ed-171">Creating Items</span></span>

<span data-ttu-id="457ed-172">По соглашению создание новых элементов данных сопоставляется с HTTP-команду POST.</span><span class="sxs-lookup"><span data-stu-id="457ed-172">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="457ed-173">`Create` Имеет метод `[HttpPost]` атрибут, применяемый к нему и принимает `ToDoItem` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="457ed-173">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="457ed-174">Поскольку `item` аргумент будет передаваться в тексте запроса POST, этот параметр снабжен `[FromBody]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="457ed-174">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="457ed-175">Внутри метода элемент проверяется на допустимость и предыдущих существование в хранилище данных и при возникновении проблем нет, он добавляется с помощью репозитория.</span><span class="sxs-lookup"><span data-stu-id="457ed-175">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it is added using the repository.</span></span> <span data-ttu-id="457ed-176">Проверка `ModelState.IsValid` выполняет [проверка модели](../mvc/models/validation.md)и необходимо сделать в каждый метод API, принимающее вводимые пользователем данные.</span><span class="sxs-lookup"><span data-stu-id="457ed-176">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

<span data-ttu-id="457ed-177">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]</span><span class="sxs-lookup"><span data-stu-id="457ed-177">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]</span></span>

<span data-ttu-id="457ed-178">Образец использует перечисление, содержащий коды ошибок, которые передаются клиенту мобильных устройств:</span><span class="sxs-lookup"><span data-stu-id="457ed-178">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

<span data-ttu-id="457ed-179">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]</span><span class="sxs-lookup"><span data-stu-id="457ed-179">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]</span></span>

<span data-ttu-id="457ed-180">Проверить, добавление новых элементов, с помощью почтальон, выбрав команду POST, предоставляющего новый объект в формате JSON в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="457ed-180">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="457ed-181">Также следует добавить указания заголовка запроса `Content-Type` из `application/json`.</span><span class="sxs-lookup"><span data-stu-id="457ed-181">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Почтальон консоль, показывающая POST и ответ](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="457ed-183">Метод возвращает вновь созданный элемент в ответе.</span><span class="sxs-lookup"><span data-stu-id="457ed-183">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="457ed-184">Обновление элементов</span><span class="sxs-lookup"><span data-stu-id="457ed-184">Updating Items</span></span>

<span data-ttu-id="457ed-185">Изменение записей происходит с помощью HTTP-запросы PUT.</span><span class="sxs-lookup"><span data-stu-id="457ed-185">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="457ed-186">За исключением этого изменения `Edit` метод практически идентичен `Create`.</span><span class="sxs-lookup"><span data-stu-id="457ed-186">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="457ed-187">Обратите внимание, что, если запись не будет найден, `Edit` действие будет возвращать `NotFound` ответов (404).</span><span class="sxs-lookup"><span data-stu-id="457ed-187">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

<span data-ttu-id="457ed-188">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]</span><span class="sxs-lookup"><span data-stu-id="457ed-188">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]</span></span>

<span data-ttu-id="457ed-189">Для тестирования с почтальон сделать команды PUT.</span><span class="sxs-lookup"><span data-stu-id="457ed-189">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="457ed-190">Обновленный объект данных указывается в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="457ed-190">Specify the updated object data in the Body of the request.</span></span>

![Почтальон консоль, показывающая PUT и ответ](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="457ed-192">Этот метод возвращает `NoContent` ответа (204) при успешном выполнении, чтобы обеспечить согласованность с существующие API.</span><span class="sxs-lookup"><span data-stu-id="457ed-192">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="457ed-193">Удаление элементов</span><span class="sxs-lookup"><span data-stu-id="457ed-193">Deleting Items</span></span>

<span data-ttu-id="457ed-194">Удаление записей выполняется путем внесения запросов на удаление службы и передавая идентификатор элемента для удаления.</span><span class="sxs-lookup"><span data-stu-id="457ed-194">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="457ed-195">Как при обновлении будет получать запросы для элементов, которые не существуют `NotFound` ответов.</span><span class="sxs-lookup"><span data-stu-id="457ed-195">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="457ed-196">В противном случае будет получить успешный запрос `NoContent` ответа (204).</span><span class="sxs-lookup"><span data-stu-id="457ed-196">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

<span data-ttu-id="457ed-197">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]</span><span class="sxs-lookup"><span data-stu-id="457ed-197">[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]</span></span>

<span data-ttu-id="457ed-198">Обратите внимание, что при тестировании функции удаления, никаких действий не требуется в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="457ed-198">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Почтальон консоль, показывающая DELETE и ответ](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="457ed-200">Общие соглашения веб-API</span><span class="sxs-lookup"><span data-stu-id="457ed-200">Common Web API Conventions</span></span>

<span data-ttu-id="457ed-201">При разработке серверных служб для вашего приложения, может потребоваться придумать согласованный набор соглашений или политики для обработки проблем с перекрестными.</span><span class="sxs-lookup"><span data-stu-id="457ed-201">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="457ed-202">Например, в службе, показанном выше, запрашивает определенных записей, которые не были найдены получил `NotFound` ответа, а не `BadRequest` ответа.</span><span class="sxs-lookup"><span data-stu-id="457ed-202">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="457ed-203">Аналогичным образом, команды, внесенные в эту службу, переданный типы привязано к модели, всегда проверяется `ModelState.IsValid` и возвращается `BadRequest` для типов Недопустимая модель.</span><span class="sxs-lookup"><span data-stu-id="457ed-203">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="457ed-204">После определения общую политику для собственные интерфейсы API можно обычно инкапсулировать его в [фильтра](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="457ed-204">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="457ed-205">Дополнительные сведения о [как инкапсулируют общую политики API в приложениях ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="457ed-205">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
