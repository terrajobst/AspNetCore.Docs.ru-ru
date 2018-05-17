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
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="4a846-103">Начало работы с NSwag и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a846-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="4a846-104">Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="4a846-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="4a846-105">Для использования [NSwag](https://github.com/RSuter/NSwag) с ПО промежуточного слоя ASP.NET Core требуется пакет NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="4a846-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="4a846-106">Пакет состоит из генератора Swagger, пользовательского интерфейса Swagger (v2 и v3) и [пользовательского интерфейса ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="4a846-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="4a846-107">Настоятельно рекомендуется использовать возможности создания кода в NSwag.</span><span class="sxs-lookup"><span data-stu-id="4a846-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="4a846-108">Выберите один из следующих вариантов создания кода:</span><span class="sxs-lookup"><span data-stu-id="4a846-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="4a846-109">Используйте [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), классическое приложение Windows для создания клиентского кода в C# и TypeScript для вашего API</span><span class="sxs-lookup"><span data-stu-id="4a846-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="4a846-110">Используйте пакеты NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) или [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), чтобы создавать код внутри проекта</span><span class="sxs-lookup"><span data-stu-id="4a846-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="4a846-111">Используйте NSwag из [командной строки](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="4a846-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="4a846-112">Используйте пакет NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)</span><span class="sxs-lookup"><span data-stu-id="4a846-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="4a846-113">Функции</span><span class="sxs-lookup"><span data-stu-id="4a846-113">Features</span></span>

<span data-ttu-id="4a846-114">Основная причина применения NSwag — возможность не только ввести пользовательский интерфейс Swagger и генератор Swagger, но и использовать гибкие возможности создания кода.</span><span class="sxs-lookup"><span data-stu-id="4a846-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="4a846-115">Вам не требуется существующий API &mdash; вы можете использовать API сторонних разработчиков, которые содержат Swagger и позволяют NSwag создавать реализацию клиента.</span><span class="sxs-lookup"><span data-stu-id="4a846-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="4a846-116">В любом случае цикл разработки ускоряется, и вам проще адаптироваться к изменениям API.</span><span class="sxs-lookup"><span data-stu-id="4a846-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="4a846-117">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="4a846-117">Package installation</span></span>

<span data-ttu-id="4a846-118">Пакет NuGet NSwag можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="4a846-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a846-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a846-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a846-120">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="4a846-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="4a846-121">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="4a846-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="4a846-122">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4a846-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="4a846-123">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="4a846-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="4a846-124">В поле поиска введите "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="4a846-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="4a846-125">Выберите пакет "NSwag.AspNetCore" на вкладке **Обзор** и нажмите **Установить**</span><span class="sxs-lookup"><span data-stu-id="4a846-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4a846-126">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4a846-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4a846-127">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="4a846-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="4a846-128">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="4a846-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="4a846-129">В поле поиска введите "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="4a846-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="4a846-130">В области результатов выберите пакет "NSwag.AspNetCore" и нажмите **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="4a846-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a846-131">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4a846-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4a846-132">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="4a846-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4a846-133">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="4a846-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4a846-134">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4a846-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="4a846-135">Добавление и настройка ПО промежуточного слоя Swagger</span><span class="sxs-lookup"><span data-stu-id="4a846-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="4a846-136">Импортируйте следующие пространства имен в класс `Info`:</span><span class="sxs-lookup"><span data-stu-id="4a846-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="4a846-137">В методе `Startup.Configure` включите ПО промежуточного слоя для обслуживания созданной спецификации Swagger и пользовательского интерфейса Swagger:</span><span class="sxs-lookup"><span data-stu-id="4a846-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="4a846-138">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="4a846-138">Launch the app.</span></span> <span data-ttu-id="4a846-139">Перейдите к `/swagger` для просмотра пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="4a846-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="4a846-140">Перейдите к `/swagger/v1/swagger.json` для просмотра спецификации Swagger.</span><span class="sxs-lookup"><span data-stu-id="4a846-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="4a846-141">Создание кода</span><span class="sxs-lookup"><span data-stu-id="4a846-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="4a846-142">Через NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="4a846-142">Via NSwagStudio</span></span>

* <span data-ttu-id="4a846-143">Установите `NSwagStudio` из официального [репозитория GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="4a846-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="4a846-144">Запустите NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="4a846-144">Launch NSwagStudio.</span></span> <span data-ttu-id="4a846-145">Введите расположение файла *swagger.json* или скопируйте его напрямую:</span><span class="sxs-lookup"><span data-stu-id="4a846-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="4a846-147">Укажите желаемый тип выходных данных клиента.</span><span class="sxs-lookup"><span data-stu-id="4a846-147">Indicate the desired client output type.</span></span> <span data-ttu-id="4a846-148">Доступные варианты: **клиент TypeScript**, **клиент CSharp** или **контроллер веб-API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="4a846-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="4a846-149">Использование контроллера веб-API по сути является обратным созданием.</span><span class="sxs-lookup"><span data-stu-id="4a846-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="4a846-150">Служба воссоздается по спецификации службы.</span><span class="sxs-lookup"><span data-stu-id="4a846-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="4a846-151">Нажмите **Создать выходные данные**.</span><span class="sxs-lookup"><span data-stu-id="4a846-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="4a846-152">Вы увидите полную клиентскую реализацию примера *TodoApi.NSwag* на языке C#:</span><span class="sxs-lookup"><span data-stu-id="4a846-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="4a846-154">Поместите файл в клиентский проект (например, приложение [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="4a846-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="4a846-155">Начните использовать API:</span><span class="sxs-lookup"><span data-stu-id="4a846-155">Start consuming the API:</span></span>

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
> <span data-ttu-id="4a846-156">Вы можете вставить базовый URL-адрес и (или) HTTP-клиент в API-клиент.</span><span class="sxs-lookup"><span data-stu-id="4a846-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="4a846-157">Рекомендуется всегда [повторно использовать HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="4a846-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="4a846-158">Теперь можно без труда приступить к реализации API в клиентских проектах.</span><span class="sxs-lookup"><span data-stu-id="4a846-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="4a846-159">Другие способы создания кода клиента</span><span class="sxs-lookup"><span data-stu-id="4a846-159">Other ways to generate client code</span></span>

<span data-ttu-id="4a846-160">Можно создать код другими способами, более подходящими для вашего рабочего процесса:</span><span class="sxs-lookup"><span data-stu-id="4a846-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="4a846-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="4a846-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="4a846-162">В коде</span><span class="sxs-lookup"><span data-stu-id="4a846-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="4a846-163">Шаблоны T4</span><span class="sxs-lookup"><span data-stu-id="4a846-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="4a846-164">Настройка</span><span class="sxs-lookup"><span data-stu-id="4a846-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="4a846-165">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="4a846-165">XML comments</span></span>

<span data-ttu-id="4a846-166">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="4a846-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="4a846-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a846-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="4a846-168">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="4a846-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="4a846-169">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**:</span><span class="sxs-lookup"><span data-stu-id="4a846-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Свойства проекта, вкладка "Сборка"](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="4a846-171">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4a846-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="4a846-172">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.</span><span class="sxs-lookup"><span data-stu-id="4a846-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="4a846-173">Установите флажок **Сформировать документацию XML** в разделе **Общие параметры**:</span><span class="sxs-lookup"><span data-stu-id="4a846-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Параметры проекта, раздел "Общие параметры"](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="4a846-175">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4a846-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="4a846-176">Вручную добавьте в файл *.csproj* следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="4a846-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="4a846-177">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="4a846-177">Data annotations</span></span>

<span data-ttu-id="4a846-178">NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="4a846-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="4a846-179">Следовательно, NSwag не удается определить, что делает ваше действие и что оно возвращает.</span><span class="sxs-lookup"><span data-stu-id="4a846-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="4a846-180">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="4a846-180">Consider the following example:</span></span>

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

<span data-ttu-id="4a846-181">Предыдущее действие возвращает `IActionResult`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) или [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="4a846-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="4a846-182">Заметки к данным используются, чтобы сообщить клиентам, какой ответ HTTP возвращает это действие.</span><span class="sxs-lookup"><span data-stu-id="4a846-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="4a846-183">Добавьте к действию следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="4a846-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="4a846-184">Генератор Swagger теперь может точно описать это действие, и созданные клиенты знают, что они получают при вызове конечной точки.</span><span class="sxs-lookup"><span data-stu-id="4a846-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="4a846-185">Настоятельно рекомендуется добавлять эти атрибуты ко всем действиям.</span><span class="sxs-lookup"><span data-stu-id="4a846-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="4a846-186">Рекомендации о том, какие ответы HTTP должны возвращать ваши действия API, см. в [спецификации RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="4a846-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
