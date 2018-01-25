---
title: "Страницы справки по веб-API ASP.NET Core с использованием Swagger"
author: spboyer
description: "В этом учебнике приводится пошаговое руководство по добавлению Swagger для составления документации и страниц справки к приложению веб-API."
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: d044c820057dba762d3a0f621855a8f4e298ab23
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="f323d-103">Страницы справки по веб-API ASP.NET Core с использованием Swagger</span><span class="sxs-lookup"><span data-stu-id="f323d-103">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="f323d-104">Авторы: [Шейн Бойер](https://twitter.com/spboyer) (Shayne Boyer) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="f323d-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f323d-105">При сборке пользовательского приложения разработчик может обнаружить, что разобраться в различных методах API не так-то просто.</span><span class="sxs-lookup"><span data-stu-id="f323d-105">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="f323d-106">Используя [Swagger](https://swagger.io/) с реализацией .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), можно получить хорошую документацию и справку по веб-API, добавив пару пакетов NuGet и скорректировав файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f323d-106">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="f323d-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) — это проект с открытым исходным кодом, предназначенный для создания документов Swagger по веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f323d-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="f323d-108">[Swagger](https://swagger.io/) — это машиночитаемое представление API RESTful, которое обеспечивает поддержку интерактивной документации, создание клиентских пакетов SDK и возможность обнаружения.</span><span class="sxs-lookup"><span data-stu-id="f323d-108">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="f323d-109">В этом учебнике используется пример из статьи [Сборка первого веб-API с использованием ASP.NET Core MVC и Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="f323d-109">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="f323d-110">Если вы хотите повторить описанные действия, загрузите этот пример со страницы [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="f323d-110">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="f323d-111">Начало работы</span><span class="sxs-lookup"><span data-stu-id="f323d-111">Getting Started</span></span>

<span data-ttu-id="f323d-112">Swashbuckle включает три основных компонента.</span><span class="sxs-lookup"><span data-stu-id="f323d-112">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="f323d-113">`Swashbuckle.AspNetCore.Swagger`: объектная модель Swagger и ПО промежуточного слоя для предоставления объектов `SwaggerDocument` как конечных точек JSON.</span><span class="sxs-lookup"><span data-stu-id="f323d-113">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="f323d-114">`Swashbuckle.AspNetCore.SwaggerGen`: генератор Swagger, создающий объекты `SwaggerDocument` непосредственно из ваших маршрутов, контроллеров и моделей.</span><span class="sxs-lookup"><span data-stu-id="f323d-114">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="f323d-115">Как правило, он комбинируется с ПО промежуточного слоя конечной точки Swagger и за счет этого предоставляет Swagger JSON автоматически.</span><span class="sxs-lookup"><span data-stu-id="f323d-115">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="f323d-116">`Swashbuckle.AspNetCore.SwaggerUI`: встроенная версия пользовательского интерфейса Swagger, которая интерпретирует Swagger JSON и обеспечивает удобную, настраиваемую среду для описания функциональности веб-API.</span><span class="sxs-lookup"><span data-stu-id="f323d-116">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="f323d-117">Включает встроенные окружения тестов для открытых методов.</span><span class="sxs-lookup"><span data-stu-id="f323d-117">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="f323d-118">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="f323d-118">NuGet Packages</span></span>

<span data-ttu-id="f323d-119">Swashbuckle можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="f323d-119">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f323d-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f323d-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f323d-121">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="f323d-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="f323d-122">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="f323d-122">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="f323d-123">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f323d-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="f323d-124">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="f323d-124">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="f323d-125">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="f323d-125">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="f323d-126">Выберите пакет "Swashbuckle.AspNetCore" на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="f323d-126">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f323d-127">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f323d-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f323d-128">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="f323d-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="f323d-129">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="f323d-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="f323d-130">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="f323d-130">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="f323d-131">В области результатов выберите пакет "Swashbuckle.AspNetCore" и нажмите кнопку **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="f323d-131">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f323d-132">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f323d-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f323d-133">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="f323d-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f323d-134">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="f323d-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f323d-135">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f323d-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="f323d-136">Добавление и настройка Swagger для ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="f323d-136">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="f323d-137">Добавьте генератор Swagger в коллекцию служб в методе `ConfigureServices` файла *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f323d-137">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="f323d-138">Добавьте следующий оператор using для класса `Info`:</span><span class="sxs-lookup"><span data-stu-id="f323d-138">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="f323d-139">В методе `Configure` файла *Startup.cs* включите ПО промежуточного слоя для обслуживания созданного документа JSON и SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="f323d-139">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="f323d-140">Запустите приложение и перейдите к файлу `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="f323d-140">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="f323d-141">Появится созданный документ с описанием конечных точек.</span><span class="sxs-lookup"><span data-stu-id="f323d-141">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="f323d-142">**Примечание**. Microsoft Edge, Google Chrome и Firefox отображают документы JSON изначально.</span><span class="sxs-lookup"><span data-stu-id="f323d-142">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="f323d-143">Существуют расширения для Chrome, которые форматируют документ для удобства чтения.</span><span class="sxs-lookup"><span data-stu-id="f323d-143">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="f323d-144">*Для краткости следующий пример приводится в сокращенном варианте.*</span><span class="sxs-lookup"><span data-stu-id="f323d-144">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

<span data-ttu-id="f323d-145">Этот документ запускает пользовательский интерфейс Swagger, для просмотра которого нужно перейти к `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="f323d-145">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Пользовательский интерфейс Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="f323d-147">Каждый метод открытого действия в `TodoController` можно протестировать в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="f323d-147">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="f323d-148">Щелкните имя метода, чтобы развернуть соответствующий раздел.</span><span class="sxs-lookup"><span data-stu-id="f323d-148">Click a method name to expand the section.</span></span> <span data-ttu-id="f323d-149">Добавьте все необходимые параметры и нажмите кнопку "Попробуйте!".</span><span class="sxs-lookup"><span data-stu-id="f323d-149">Add any necessary parameters, and click "Try it out!".</span></span>

![Пример тестирования в Swagger метода GET](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="f323d-151">Настройка и расширяемость</span><span class="sxs-lookup"><span data-stu-id="f323d-151">Customization & Extensibility</span></span>

<span data-ttu-id="f323d-152">Swagger предоставляет параметры для документирования объектной модели и настройки пользовательского интерфейса в соответствии с вашим фирменным стилем.</span><span class="sxs-lookup"><span data-stu-id="f323d-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="f323d-153">Данные и описание API</span><span class="sxs-lookup"><span data-stu-id="f323d-153">API Info and Description</span></span>

<span data-ttu-id="f323d-154">Действия по настройке, передаваемые в метод `AddSwaggerGen`, можно использовать для добавления таких сведений, как автор, лицензия и описание:</span><span class="sxs-lookup"><span data-stu-id="f323d-154">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="f323d-155">Ниже показан фрагмент пользовательского интерфейса Swagger, в котором отображаются сведения о версии.</span><span class="sxs-lookup"><span data-stu-id="f323d-155">The following image depicts the Swagger UI displaying the version information:</span></span>

![Пользовательский интерфейс Swagger с данными версии: описание, автор и ссылка на дополнительные сведения](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="f323d-157">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="f323d-157">XML Comments</span></span>

<span data-ttu-id="f323d-158">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f323d-158">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f323d-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f323d-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f323d-160">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="f323d-160">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="f323d-161">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**:</span><span class="sxs-lookup"><span data-stu-id="f323d-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Свойства проекта, вкладка "Сборка"](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f323d-163">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f323d-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f323d-164">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.</span><span class="sxs-lookup"><span data-stu-id="f323d-164">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="f323d-165">Установите флажок **Сформировать документацию XML** в разделе **Общие параметры**:</span><span class="sxs-lookup"><span data-stu-id="f323d-165">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Параметры проекта, раздел "Общие параметры"](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f323d-167">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f323d-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f323d-168">Вручную добавьте в файл *.csproj* следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="f323d-168">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f323d-169">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="f323d-169">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f323d-170">См. описание Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f323d-170">See Visual Studio Code.</span></span>

---

<span data-ttu-id="f323d-171">Включение комментариев XML предоставляет отладочную информацию для недокументированных открытых типов и членов.</span><span class="sxs-lookup"><span data-stu-id="f323d-171">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="f323d-172">Недокументированные типы и члены обозначены предупреждающим сообщением: *Отсутствует комментарий XML для открытого видимого типа или члена*.</span><span class="sxs-lookup"><span data-stu-id="f323d-172">Undocumented types and members are indicated by the warning message: *Missing XML comment for publicly visible type or member*.</span></span>

<span data-ttu-id="f323d-173">Настройте Swagger на использование созданного XML-файла.</span><span class="sxs-lookup"><span data-stu-id="f323d-173">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="f323d-174">В Linux и других операционных системах, отличных от Windows, имена и пути файлов могут быть чувствительны к регистру.</span><span class="sxs-lookup"><span data-stu-id="f323d-174">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="f323d-175">Например, файл *ToDoApi.XML* будет найден в Windows, но не в CentOS.</span><span class="sxs-lookup"><span data-stu-id="f323d-175">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="f323d-176">В представленном выше коде `ApplicationBasePath` получает базовый путь приложения.</span><span class="sxs-lookup"><span data-stu-id="f323d-176">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="f323d-177">Базовый путь используется для поиска файла XML-комментариев.</span><span class="sxs-lookup"><span data-stu-id="f323d-177">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="f323d-178">*TodoApi.xml* работает только в этом примере, так как имя созданного файла комментариев XML основано на имени приложения.</span><span class="sxs-lookup"><span data-stu-id="f323d-178">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="f323d-179">Включение в метод комментариев с тройной косой чертой улучшает пользовательский интерфейс Swagger, так как позволяет добавить описание к заголовку раздела:</span><span class="sxs-lookup"><span data-stu-id="f323d-179">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Пользовательский интерфейс Swagger с XML-комментарием "Удаляет указанный элемент TodoItem"](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="f323d-182">Пользовательский интерфейс определяется созданным файлом JSON, который также содержит следующие комментарии.</span><span class="sxs-lookup"><span data-stu-id="f323d-182">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="f323d-183">Добавьте тег [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) в документацию к методу действия `Create`.</span><span class="sxs-lookup"><span data-stu-id="f323d-183">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="f323d-184">Он дополняет сведения, указанные в теге `<summary>`, и предоставляет более надежный пользовательский интерфейс Swagger.</span><span class="sxs-lookup"><span data-stu-id="f323d-184">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="f323d-185">Содержимое тега `<remarks>` может включать текст, JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="f323d-185">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="f323d-186">Обратите внимание, как эти дополнительные комментарии улучшают пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="f323d-186">Notice the UI enhancements with these additional comments.</span></span>

![Пользовательский интерфейс Swagger с показанными дополнительными комментариями](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="f323d-188">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="f323d-188">Data Annotations</span></span>

<span data-ttu-id="f323d-189">Добавьте к модели атрибуты из `System.ComponentModel.DataAnnotations`, чтобы упростить реализацию компонентов пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="f323d-189">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="f323d-190">Добавьте атрибут `[Required]` к свойству `Name` класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="f323d-190">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="f323d-191">Наличие этого атрибута изменяет поведение пользовательского интерфейса и схему базового JSON.</span><span class="sxs-lookup"><span data-stu-id="f323d-191">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="f323d-192">Добавьте к контроллеру API атрибут `[Produces("application/json")]`.</span><span class="sxs-lookup"><span data-stu-id="f323d-192">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="f323d-193">Он позволяет объявить, что действия контроллера поддерживают возврат типа содержимого *application/json*:</span><span class="sxs-lookup"><span data-stu-id="f323d-193">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="f323d-194">В раскрывающемся списке **Тип содержимого ответа** этот тип содержимого выбирается по умолчанию для операций контроллера GET.</span><span class="sxs-lookup"><span data-stu-id="f323d-194">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Пользовательский интерфейс Swagger с типом содержимого ответа по умолчанию](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="f323d-196">Чем больше заметок к данным используется в веб-API, тем более содержательными и полезными становятся пользовательский интерфейс и страницы справки по API.</span><span class="sxs-lookup"><span data-stu-id="f323d-196">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="f323d-197">Описания типов ответов</span><span class="sxs-lookup"><span data-stu-id="f323d-197">Describing Response Types</span></span>

<span data-ttu-id="f323d-198">Разработчиков пользовательских приложений в первую очередь волнуют возвращаемые данные &mdash; а именно типы ответов и коды ошибок (если они не стандартны).</span><span class="sxs-lookup"><span data-stu-id="f323d-198">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="f323d-199">Все это указывается в XML-комментарии и заметках к данным.</span><span class="sxs-lookup"><span data-stu-id="f323d-199">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="f323d-200">Действие `Create` возвращает `201 Created` в случае успеха и `400 Bad Request`, если текст опубликованного запроса пустой.</span><span class="sxs-lookup"><span data-stu-id="f323d-200">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="f323d-201">Без надлежащей документации в пользовательском интерфейсе Swagger пользователь не будет знать, чего ожидать.</span><span class="sxs-lookup"><span data-stu-id="f323d-201">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="f323d-202">Эту проблему решает добавление строк, выделенных в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="f323d-202">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="f323d-203">Теперь пользовательский интерфейс Swagger четко документирует ожидаемые коды HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="f323d-203">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Пользовательский интерфейс Swagger, показывающий описание класса ответа POST "Возвращает вновь созданный элемент Todo" и "400 — Если элемент имеет значение NULL" для кода состояния и причины в разделе "Сообщения ответов"](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="f323d-205">Настройка пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="f323d-205">Customizing the UI</span></span>

<span data-ttu-id="f323d-206">Стандартный пользовательский интерфейс функционален и представителен, но при сборке документации для своего API вы, скорее всего, захотите отобразить свой бренд или фирменный стиль.</span><span class="sxs-lookup"><span data-stu-id="f323d-206">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="f323d-207">Для решения этой задачи с использованием компонентов Swashbuckle необходимо добавить ресурсы для обслуживания статических файлов, а затем построить структуру папок для их размещения.</span><span class="sxs-lookup"><span data-stu-id="f323d-207">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="f323d-208">Если документация создается для платформы .NET Framework, добавьте в проект пакет NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="f323d-208">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="f323d-209">Включите ПО промежуточного слоя для статических файлов:</span><span class="sxs-lookup"><span data-stu-id="f323d-209">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="f323d-210">Получите содержимое папки *dist* из [репозитория GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="f323d-210">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="f323d-211">Эта папка содержит ресурсы, необходимые для страниц пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="f323d-211">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="f323d-212">Создайте папку *wwwroot/swagger/ui* и скопируйте в нее содержимое папки *dist*.</span><span class="sxs-lookup"><span data-stu-id="f323d-212">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="f323d-213">Создайте файл *wwwroot/swagger/ui/css/custom.css* со следующим кодом CSS для настройки заголовка страницы:</span><span class="sxs-lookup"><span data-stu-id="f323d-213">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="f323d-214">Сошлитесь на файл *custom.css* в файле *index.html*:</span><span class="sxs-lookup"><span data-stu-id="f323d-214">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="f323d-215">Перейдите на страницу *index.html* в `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="f323d-215">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="f323d-216">Введите `http://localhost:<random_port>/swagger/v1/swagger.json` в текстовое поле заголовка и нажмите кнопку **Проводник**.</span><span class="sxs-lookup"><span data-stu-id="f323d-216">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="f323d-217">Полученная в итоге страница будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f323d-217">The resulting page looks as follows:</span></span>

![Пользовательский интерфейс Swagger с настроенным заголовком](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="f323d-219">С этой страницей можно сделать гораздо больше.</span><span class="sxs-lookup"><span data-stu-id="f323d-219">There is much more you can do with the page.</span></span> <span data-ttu-id="f323d-220">Все возможности ресурсов пользовательского интерфейса см. в статье о [репозитории GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="f323d-220">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
