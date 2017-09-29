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
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Создание серверных служб для собственных мобильных приложений

По [Стив Смит](https://ardalis.com/)

Мобильные приложения могут легко взаимодействовать с ASP.NET Core серверных служб.

[Просмотреть или загрузить образец кода службы серверной части](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Пример собственного мобильного приложения

Демонстрирует создание серверных служб с помощью ASP.NET MVC основных компонентов для поддержки собственных мобильных приложений. Она использует [приложения Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) как его собственного клиента, которая содержит отдельные собственных клиентов для устройств Android, iOS, универсальное приложение для Windows и Windows Phone. Можно следовать связанного учебника, чтобы создать собственное приложение (и установить необходимые бесплатные средства Xamarin), а также Загрузка примера решения Xamarin. Пример Xamarin включает проекте служб ASP.NET Web API 2 приложения ASP.NET Core в этой статье заменяет (нет изменений, требуемых клиентом).

![Выполните остальные приложению, выполняющемуся на смартфоне Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Функции

Приложение ToDoRest поддерживает перечисление, добавление, удаление и обновление элементов задач. Каждый элемент имеет идентификатор, имя, заметки и свойство, указывающее, было ли оно уже выполняется еще.

Основного представления элементов, как показано выше, выводит список имен каждого элемента и указывает, если он выполняется с флажком.

Коснувшись `+` значок открывает диалоговое окно добавления элемента:

![Диалоговое окно добавления элемента](native-mobile-backend/_static/todo-android-new-item.png)

При касании элемента на экране основного списка откроется диалоговое окно редактирования, где можно изменить имя элемента, заметки и Готово параметры, или элемент может быть удален:

![Изменение элемента диалогового окна](native-mobile-backend/_static/todo-android-edit-item.png)

В этом примере настраивается по умолчанию для использования серверных служб, размещенных на developer.xamarin.com, которые позволяет выполнять операции только для чтения. Можно протестировать их самостоятельно для приложения ASP.NET Core, созданные в следующем разделе, запущенными на локальном компьютере, необходимо обновить приложение `RestUrl` константой. Перейдите к `ToDoREST` проект и откройте *Constants.cs* файла. Замените `RestUrl` URL-адрес, включающий компьютера IP адрес (не "localhost" или "127.0.0.1, так как этот адрес используется в эмуляторе устройства не со своего компьютера). Включите имя порта (5000). Для тестирования работы служб с устройством, убедитесь, что у вас нет активных брандмауэр блокирует доступ к этому порту.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Создание проекта ASP.NET Core

Создайте новое веб-приложение ASP.NET Core в Visual Studio. Выберите шаблон веб-API и без проверки подлинности. Назовите проект *ToDoApi*.

![Диалоговое окно создания веб-приложения ASP.NET с выбранного шаблона проекта веб-API](native-mobile-backend/_static/web-api-template.png)

Приложение должно отвечать на все запросы, адресованные port 5000. Обновление *Program.cs* для включения `.UseUrls("http://*:5000")` для этого:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Убедитесь, что приложение запущено непосредственно, а не за IIS Express, которое не обрабатывает запросы нелокальных по умолчанию. Запустите `dotnet run` из командной строки, или выберите имя профиля приложения в раскрывающемся списке целевой объект отладки в панели инструментов Visual Studio.

Добавьте класс модели для представления элементов задач. Пометить необходимые поля с помощью `[Required]` атрибута:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Методы API требуется какой-либо способ работы с данными. Используйте тот же `IToDoRepository` интерфейс исходные образцы использования Xamarin:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

В этом образце реализация использует только частной коллекции элементов:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Настройте реализацию в *файла Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

На этом этапе вы будете готовы создать *ToDoItemsController*.

> [!TIP]
> Узнайте больше о создании веб-API в [построение свой первый веб-API с Visual Studio и ASP.NET Core MVC](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Создание контроллера

Добавьте в проект новый контроллер *ToDoItemsController*. Он должен наследовать от Microsoft.AspNetCore.Mvc.Controller. Добавить `Route` атрибут, чтобы указать, что контроллер будет обрабатывать запросы, адресованные пути, начинающиеся с `api/todoitems`. `[controller]` В маршруте заменяется имя контроллера (пропуск `Controller` суффикс) и особенно полезно для глобального маршрутов. Дополнительные сведения о [маршрутизации](../fundamentals/routing.md).

Роли контроллера требуется `IToDoRepository` для функции; запроса экземпляра этого типа через конструктор контроллера. Во время выполнения, этот экземпляр будет предоставляться с использованием поддержки framework [внедрения зависимостей](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Этот API поддерживает четыре различных HTTP-команд для выполнения операций CRUD (Создание, чтение, обновление, удаление) в источнике данных. Самый простой из них является операции чтения, которая соответствует запрос HTTP GET.

### <a name="reading-items"></a>Чтение элементов

Запрашивает список элементов, выполняется с помощью запроса GET к `List` метод. `[HttpGet]` Атрибут `List` метод указывает, что это действие только должна обрабатывать запросы GET. Маршрут для этого действия будет маршрута, указанного на контроллере. Не обязательно будет использовать имя действия в рамках маршрута. Необходимо убедиться, что каждое действие имеет уникальный и однозначный маршрута. Атрибуты маршрутизации может применяться в контроллер и уровни метод для создания определенных маршрутов.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` Метод возвращает код ответа 200 ОК и все элементы ToDo, сериализованным в JSON.

Можно проверить новый метод API с помощью различных средств, таких как [почтальон](https://www.getpostman.com/docs/), показано ниже:

![Почтальон консоль, показывающая запрос GET для todoitems и текст ответа, показывающая JSON для трех элементов, возвращенных](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Создание элементов

По соглашению создание новых элементов данных сопоставляется с HTTP-команду POST. `Create` Имеет метод `[HttpPost]` атрибут, применяемый к нему и принимает `ToDoItem` экземпляра. Поскольку `item` аргумент будет передаваться в тексте запроса POST, этот параметр снабжен `[FromBody]` атрибута.

Внутри метода элемент проверяется на допустимость и предыдущих существование в хранилище данных и при возникновении проблем нет, он добавляется с помощью репозитория. Проверка `ModelState.IsValid` выполняет [проверка модели](../mvc/models/validation.md)и необходимо сделать в каждый метод API, принимающее вводимые пользователем данные.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

Образец использует перечисление, содержащий коды ошибок, которые передаются клиенту мобильных устройств:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Проверить, добавление новых элементов, с помощью почтальон, выбрав команду POST, предоставляющего новый объект в формате JSON в теле запроса. Также следует добавить указания заголовка запроса `Content-Type` из `application/json`.

![Почтальон консоль, показывающая POST и ответ](native-mobile-backend/_static/postman-post.png)

Метод возвращает вновь созданный элемент в ответе.

### <a name="updating-items"></a>Обновление элементов

Изменение записей происходит с помощью HTTP-запросы PUT. За исключением этого изменения `Edit` метод практически идентичен `Create`. Обратите внимание, что, если запись не будет найден, `Edit` действие будет возвращать `NotFound` ответов (404).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Для тестирования с почтальон сделать команды PUT. Обновленный объект данных указывается в тексте запроса.

![Почтальон консоль, показывающая PUT и ответ](native-mobile-backend/_static/postman-put.png)

Этот метод возвращает `NoContent` ответа (204) при успешном выполнении, чтобы обеспечить согласованность с существующие API.

### <a name="deleting-items"></a>Удаление элементов

Удаление записей выполняется путем внесения запросов на удаление службы и передавая идентификатор элемента для удаления. Как при обновлении будет получать запросы для элементов, которые не существуют `NotFound` ответов. В противном случае будет получить успешный запрос `NoContent` ответа (204).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Обратите внимание, что при тестировании функции удаления, никаких действий не требуется в теле запроса.

![Почтальон консоль, показывающая DELETE и ответ](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Общие соглашения веб-API

При разработке серверных служб для вашего приложения, может потребоваться придумать согласованный набор соглашений или политики для обработки проблем с перекрестными. Например, в службе, показанном выше, запрашивает определенных записей, которые не были найдены получил `NotFound` ответа, а не `BadRequest` ответа. Аналогичным образом, команды, внесенные в эту службу, переданный типы привязано к модели, всегда проверяется `ModelState.IsValid` и возвращается `BadRequest` для типов Недопустимая модель.

После определения общую политику для собственные интерфейсы API можно обычно инкапсулировать его в [фильтра](../mvc/controllers/filters.md). Дополнительные сведения о [как инкапсулируют общую политики API в приложениях ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).
