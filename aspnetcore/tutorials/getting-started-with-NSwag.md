---
title: Начало работы с NSwag и ASP.NET Core
author: zuckerthoben
description: Узнайте, как использовать NSwag для создания документации и страниц справки для веб-приложения API в ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Начало работы с NSwag и ASP.NET Core

Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](https://rsuter.com)

Для использования [NSwag](https://github.com/RSuter/NSwag) с ПО промежуточного слоя ASP.NET Core требуется пакет NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Пакет состоит из генератора Swagger, пользовательского интерфейса Swagger (v2 и v3) и [пользовательского интерфейса ReDoc](https://github.com/Rebilly/ReDoc).

Настоятельно рекомендуется использовать возможности создания кода в NSwag. Выберите один из следующих вариантов создания кода:

* Используйте [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), классическое приложение Windows для создания клиентского кода в C# и TypeScript для вашего API
* Используйте пакеты NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) или [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), чтобы создавать код внутри проекта
* Используйте NSwag из [командной строки](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Используйте пакет NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)

## <a name="features"></a>Функции

Основная причина применения NSwag — возможность не только ввести пользовательский интерфейс Swagger и генератор Swagger, но и использовать гибкие возможности создания кода. Вам не требуется существующий API &mdash; вы можете использовать API сторонних разработчиков, которые содержат Swagger и позволяют NSwag создавать реализацию клиента. В любом случае цикл разработки ускоряется, и вам проще адаптироваться к изменениям API.

## <a name="package-installation"></a>Установка пакета

Пакет NuGet NSwag можно добавить одним из описанных ниже способов.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В окне **Консоль диспетчера пакетов**

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* В диалоговом окне **Управление пакетами NuGet**
  * Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.
  * В качестве **источника пакета** выберите "nuget.org".
  * В поле поиска введите "NSwag.AspNetCore"
  * Выберите пакет "NSwag.AspNetCore" на вкладке **Обзор** и нажмите **Установить**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.
* В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".
* В поле поиска введите "NSwag.AspNetCore"
* В области результатов выберите пакет "NSwag.AspNetCore" и нажмите **Добавить пакет**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Во **встроенном терминале** выполните следующую команду.

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Добавление и настройка ПО промежуточного слоя Swagger

Импортируйте следующие пространства имен в класс `Info`:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

В методе `Startup.Configure` включите ПО промежуточного слоя для обслуживания созданной спецификации Swagger и пользовательского интерфейса Swagger:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Запустите приложение. Перейдите к `/swagger` для просмотра пользовательского интерфейса Swagger. Перейдите к `/swagger/v1/swagger.json` для просмотра спецификации Swagger.

## <a name="code-generation"></a>Создание кода

### <a name="via-nswagstudio"></a>Через NSwagStudio

* Установите `NSwagStudio` из официального [репозитория GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).

* Запустите NSwagStudio. Введите расположение файла *swagger.json* или скопируйте его напрямую:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Укажите желаемый тип выходных данных клиента. Доступные варианты: **клиент TypeScript**, **клиент CSharp** или **контроллер веб-API CSharp**. Использование контроллера веб-API по сути является обратным созданием. Служба воссоздается по спецификации службы.

* Нажмите **Создать выходные данные**.

* Вы увидите полную клиентскую реализацию примера *TodoApi.NSwag* на языке C#:

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Поместите файл в клиентский проект (например, приложение [Xamarin.Forms](/xamarin/xamarin-forms/)). Начните использовать API:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Вы можете вставить базовый URL-адрес и (или) HTTP-клиент в API-клиент. Рекомендуется всегда [повторно использовать HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

Теперь можно без труда приступить к реализации API в клиентских проектах.

### <a name="other-ways-to-generate-client-code"></a>Другие способы создания кода клиента

Можно создать код другими способами, более подходящими для вашего рабочего процесса:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [В коде](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Шаблоны T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Настройка

### <a name="xml-comments"></a>XML-комментарии

XML-комментарии можно включить следующим образом.

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.
* Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**:

![Свойства проекта, вкладка "Сборка"](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio для Mac](#tab/visual-studio-mac-xml/)
* Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.
* Установите флажок **Сформировать документацию XML** в разделе **Общие параметры**:

![Параметры проекта, раздел "Общие параметры"](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code.](#tab/visual-studio-code-xml/)
Вручную добавьте в файл *.csproj* следующий фрагмент кода:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Заметки к данным

NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Следовательно, NSwag не удается определить, что делает ваше действие и что оно возвращает. Рассмотрим следующий пример.

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

Предыдущее действие возвращает `IActionResult`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) или [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Заметки к данным используются, чтобы сообщить клиентам, какой ответ HTTP возвращает это действие. Добавьте к действию следующие атрибуты:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Генератор Swagger теперь может точно описать это действие, и созданные клиенты знают, что они получают при вызове конечной точки. Настоятельно рекомендуется добавлять эти атрибуты ко всем действиям. Рекомендации о том, какие ответы HTTP должны возвращать ваши действия API, см. в [спецификации RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
