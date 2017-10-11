---
title: "Страницы справки по веб-API ASP.NET Core с использованием Swagger"
author: spboyer
description: "В этом учебнике приводится пошаговое руководство по добавлению Swagger для составления документации и страниц справки к приложению веб-API."
keywords: "ASP.NET Core, Swagger, Swashbuckle, страницы справки, веб-API"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 7eeb8f0517b8806cabdd59e7d81f8c2272238615
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="aspnet-web-api-help-pages-using-swagger"></a><span data-ttu-id="15e93-104">Страницы справки по веб-API ASP.NET Core с использованием Swagger</span><span class="sxs-lookup"><span data-stu-id="15e93-104">ASP.NET Web API Help Pages using Swagger</span></span>

<a name=web-api-help-pages-using-swagger></a>

<span data-ttu-id="15e93-105">Авторы: [Шейн Бойер](https://twitter.com/spboyer) (Shayne Boyer) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="15e93-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="15e93-106">При сборке пользовательского приложения разработчик может обнаружить, что разобраться в различных методах API не так-то просто.</span><span class="sxs-lookup"><span data-stu-id="15e93-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="15e93-107">Используя [Swagger](https://swagger.io/) с реализацией .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), можно получить хорошую документацию и справку по веб-API, добавив пару пакетов NuGet и скорректировав файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="15e93-107">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="15e93-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) — это проект с открытым исходным кодом, предназначенный для создания документов Swagger по веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15e93-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="15e93-109">[Swagger](https://swagger.io/) — это машиночитаемое представление API RESTful, которое обеспечивает поддержку интерактивной документации, создание клиентских пакетов SDK и возможность обнаружения.</span><span class="sxs-lookup"><span data-stu-id="15e93-109">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="15e93-110">В этом учебнике используется пример из статьи [Сборка первого веб-API с использованием ASP.NET Core MVC и Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="15e93-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="15e93-111">Если вы хотите повторить описанные действия, загрузите этот пример со страницы [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="15e93-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="15e93-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="15e93-112">Getting Started</span></span>

<span data-ttu-id="15e93-113">Swashbuckle включает три основных компонента.</span><span class="sxs-lookup"><span data-stu-id="15e93-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="15e93-114">`Swashbuckle.AspNetCore.Swagger`: объектная модель Swagger и ПО промежуточного слоя для предоставления объектов `SwaggerDocument` как конечных точек JSON.</span><span class="sxs-lookup"><span data-stu-id="15e93-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="15e93-115">`Swashbuckle.AspNetCore.SwaggerGen`: генератор Swagger, создающий объекты `SwaggerDocument` непосредственно из ваших маршрутов, контроллеров и моделей.</span><span class="sxs-lookup"><span data-stu-id="15e93-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="15e93-116">Как правило, он комбинируется с ПО промежуточного слоя конечной точки Swagger и за счет этого предоставляет Swagger JSON автоматически.</span><span class="sxs-lookup"><span data-stu-id="15e93-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="15e93-117">`Swashbuckle.AspNetCore.SwaggerUI`: встроенная версия пользовательского интерфейса Swagger, которая интерпретирует Swagger JSON и обеспечивает удобную, настраиваемую среду для описания функциональности веб-API.</span><span class="sxs-lookup"><span data-stu-id="15e93-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="15e93-118">Включает встроенные окружения тестов для открытых методов.</span><span class="sxs-lookup"><span data-stu-id="15e93-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="15e93-119">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="15e93-119">NuGet Packages</span></span>

<span data-ttu-id="15e93-120">Swashbuckle можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="15e93-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15e93-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15e93-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="15e93-122">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="15e93-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="15e93-123">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="15e93-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="15e93-124">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="15e93-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="15e93-125">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="15e93-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="15e93-126">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="15e93-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="15e93-127">Выберите пакет "Swashbuckle.AspNetCore" на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="15e93-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15e93-128">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="15e93-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="15e93-129">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="15e93-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="15e93-130">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="15e93-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="15e93-131">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="15e93-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="15e93-132">В области результатов выберите пакет "Swashbuckle.AspNetCore" и нажмите кнопку **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="15e93-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15e93-133">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="15e93-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="15e93-134">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="15e93-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15e93-135">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="15e93-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="15e93-136">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="15e93-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="15e93-137">Добавление и настройка Swagger для ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="15e93-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="15e93-138">Добавьте генератор Swagger в коллекцию служб в методе `ConfigureServices` файла *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="15e93-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="15e93-139">Добавьте следующий оператор using для класса `Info`:</span><span class="sxs-lookup"><span data-stu-id="15e93-139">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="15e93-140">В методе `Configure` файла *Startup.cs* включите ПО промежуточного слоя для обслуживания созданного документа JSON и SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="15e93-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="15e93-141">Запустите приложение и перейдите к файлу `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="15e93-141">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="15e93-142">Появится созданный документ с описанием конечных точек.</span><span class="sxs-lookup"><span data-stu-id="15e93-142">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="15e93-143">**Примечание**. Microsoft Edge, Google Chrome и Firefox отображают документы JSON изначально.</span><span class="sxs-lookup"><span data-stu-id="15e93-143">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="15e93-144">Существуют расширения для Chrome, которые форматируют документ для удобства чтения.</span><span class="sxs-lookup"><span data-stu-id="15e93-144">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="15e93-145">*Для краткости следующий пример приводится в сокращенном варианте.*</span><span class="sxs-lookup"><span data-stu-id="15e93-145">*The following example is reduced for brevity.*</span></span>

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

<span data-ttu-id="15e93-146">Этот документ запускает пользовательский интерфейс Swagger, для просмотра которого нужно перейти к `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="15e93-146">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Пользовательский интерфейс Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="15e93-148">Каждый метод открытого действия в `TodoController` можно протестировать в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="15e93-148">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="15e93-149">Щелкните имя метода, чтобы развернуть соответствующий раздел.</span><span class="sxs-lookup"><span data-stu-id="15e93-149">Click a method name to expand the section.</span></span> <span data-ttu-id="15e93-150">Добавьте все необходимые параметры и нажмите кнопку "Попробуйте!".</span><span class="sxs-lookup"><span data-stu-id="15e93-150">Add any necessary parameters, and click "Try it out!".</span></span>

![Пример тестирования в Swagger метода GET](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="15e93-152">Настройка и расширяемость</span><span class="sxs-lookup"><span data-stu-id="15e93-152">Customization & Extensibility</span></span>

<span data-ttu-id="15e93-153">Swagger предоставляет параметры для документирования объектной модели и настройки пользовательского интерфейса в соответствии с вашим фирменным стилем.</span><span class="sxs-lookup"><span data-stu-id="15e93-153">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="15e93-154">Данные и описание API</span><span class="sxs-lookup"><span data-stu-id="15e93-154">API Info and Description</span></span>

<span data-ttu-id="15e93-155">Действия по настройке, передаваемые в метод `AddSwaggerGen`, можно использовать для добавления таких сведений, как автор, лицензия и описание:</span><span class="sxs-lookup"><span data-stu-id="15e93-155">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="15e93-156">Ниже показан фрагмент пользовательского интерфейса Swagger, в котором отображаются сведения о версии.</span><span class="sxs-lookup"><span data-stu-id="15e93-156">The following image depicts the Swagger UI displaying the version information:</span></span>

![Пользовательский интерфейс Swagger с данными версии: описание, автор и ссылка на дополнительные сведения](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="15e93-158">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="15e93-158">XML Comments</span></span>

<span data-ttu-id="15e93-159">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="15e93-159">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15e93-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15e93-160">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="15e93-161">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="15e93-161">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="15e93-162">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**:</span><span class="sxs-lookup"><span data-stu-id="15e93-162">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Свойства проекта, вкладка "Сборка"](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15e93-164">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="15e93-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="15e93-165">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.</span><span class="sxs-lookup"><span data-stu-id="15e93-165">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="15e93-166">Установите флажок **Сформировать документацию XML** в разделе **Общие параметры**:</span><span class="sxs-lookup"><span data-stu-id="15e93-166">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Параметры проекта, раздел "Общие параметры"](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15e93-168">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="15e93-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="15e93-169">Вручную добавьте в файл *.csproj* следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="15e93-169">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

<span data-ttu-id="15e93-170">Настройте Swagger на использование созданного XML-файла.</span><span class="sxs-lookup"><span data-stu-id="15e93-170">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="15e93-171">В Linux и других операционных системах, отличных от Windows, имена и пути файлов могут быть чувствительны к регистру.</span><span class="sxs-lookup"><span data-stu-id="15e93-171">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="15e93-172">Например, файл *ToDoApi.XML* будет найден в Windows, но не в CentOS.</span><span class="sxs-lookup"><span data-stu-id="15e93-172">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="15e93-173">В представленном выше коде `ApplicationBasePath` получает базовый путь приложения.</span><span class="sxs-lookup"><span data-stu-id="15e93-173">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="15e93-174">Базовый путь используется для поиска файла XML-комментариев.</span><span class="sxs-lookup"><span data-stu-id="15e93-174">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="15e93-175">*TodoApi.xml* работает только в этом примере, так как имя созданного файла комментариев XML основано на имени приложения.</span><span class="sxs-lookup"><span data-stu-id="15e93-175">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="15e93-176">Включение в метод комментариев с тройной косой чертой улучшает пользовательский интерфейс Swagger, так как позволяет добавить описание к заголовку раздела:</span><span class="sxs-lookup"><span data-stu-id="15e93-176">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Пользовательский интерфейс Swagger с XML-комментарием "Удаляет указанный элемент TodoItem"](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="15e93-179">Пользовательский интерфейс определяется созданным файлом JSON, который также содержит следующие комментарии.</span><span class="sxs-lookup"><span data-stu-id="15e93-179">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="15e93-180">Добавьте тег [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) в документацию к методу действия `Create`.</span><span class="sxs-lookup"><span data-stu-id="15e93-180">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="15e93-181">Он дополняет сведения, указанные в теге `<summary>`, и предоставляет более надежный пользовательский интерфейс Swagger.</span><span class="sxs-lookup"><span data-stu-id="15e93-181">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="15e93-182">Содержимое тега `<remarks>` может включать текст, JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="15e93-182">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="15e93-183">Обратите внимание, как эти дополнительные комментарии улучшают пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="15e93-183">Notice the UI enhancements with these additional comments.</span></span>

![Пользовательский интерфейс Swagger с показанными дополнительными комментариями](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="15e93-185">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="15e93-185">Data Annotations</span></span>

<span data-ttu-id="15e93-186">Добавьте к модели атрибуты из `System.ComponentModel.DataAnnotations`, чтобы упростить реализацию компонентов пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="15e93-186">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="15e93-187">Добавьте атрибут `[Required]` к свойству `Name` класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="15e93-187">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="15e93-188">Наличие этого атрибута изменяет поведение пользовательского интерфейса и схему базового JSON.</span><span class="sxs-lookup"><span data-stu-id="15e93-188">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="15e93-189">Добавьте к контроллеру API атрибут `[Produces("application/json")]`.</span><span class="sxs-lookup"><span data-stu-id="15e93-189">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="15e93-190">Он позволяет объявить, что действия контроллера поддерживают возврат типа содержимого *application/json*:</span><span class="sxs-lookup"><span data-stu-id="15e93-190">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="15e93-191">В раскрывающемся списке **Тип содержимого ответа** этот тип содержимого выбирается по умолчанию для операций контроллера GET.</span><span class="sxs-lookup"><span data-stu-id="15e93-191">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Пользовательский интерфейс Swagger с типом содержимого ответа по умолчанию](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="15e93-193">Чем больше заметок к данным используется в веб-API, тем более содержательными и полезными становятся пользовательский интерфейс и страницы справки по API.</span><span class="sxs-lookup"><span data-stu-id="15e93-193">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="15e93-194">Описания типов ответов</span><span class="sxs-lookup"><span data-stu-id="15e93-194">Describing Response Types</span></span>

<span data-ttu-id="15e93-195">Разработчиков пользовательских приложений в первую очередь волнуют возвращаемые данные &mdash; а именно типы ответов и коды ошибок (если они не стандартны).</span><span class="sxs-lookup"><span data-stu-id="15e93-195">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="15e93-196">Все это указывается в XML-комментарии и заметках к данным.</span><span class="sxs-lookup"><span data-stu-id="15e93-196">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="15e93-197">Действие `Create` возвращает `201 Created` в случае успеха и `400 Bad Request`, если текст опубликованного запроса пустой.</span><span class="sxs-lookup"><span data-stu-id="15e93-197">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="15e93-198">Без надлежащей документации в пользовательском интерфейсе Swagger пользователь не будет знать, чего ожидать.</span><span class="sxs-lookup"><span data-stu-id="15e93-198">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="15e93-199">Эту проблему решает добавление строк, выделенных в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="15e93-199">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="15e93-200">Теперь пользовательский интерфейс Swagger четко документирует ожидаемые коды HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="15e93-200">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Пользовательский интерфейс Swagger, показывающий описание класса ответа POST "Возвращает вновь созданный элемент Todo" и "400 — Если элемент имеет значение NULL" для кода состояния и причины в разделе "Сообщения ответов"](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="15e93-202">Настройка пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="15e93-202">Customizing the UI</span></span>

<span data-ttu-id="15e93-203">Стандартный пользовательский интерфейс функционален и представителен, но при сборке документации для своего API вы, скорее всего, захотите отобразить свой бренд или фирменный стиль.</span><span class="sxs-lookup"><span data-stu-id="15e93-203">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="15e93-204">Для решения этой задачи с использованием компонентов Swashbuckle необходимо добавить ресурсы для обслуживания статических файлов, а затем построить структуру папок для их размещения.</span><span class="sxs-lookup"><span data-stu-id="15e93-204">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="15e93-205">Если документация создается для платформы .NET Framework, добавьте в проект пакет NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="15e93-205">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="15e93-206">Включите ПО промежуточного слоя для статических файлов:</span><span class="sxs-lookup"><span data-stu-id="15e93-206">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="15e93-207">Получите содержимое папки *dist* из [репозитория GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="15e93-207">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="15e93-208">Эта папка содержит ресурсы, необходимые для страниц пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="15e93-208">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="15e93-209">Создайте папку *wwwroot/swagger/ui* и скопируйте в нее содержимое папки *dist*.</span><span class="sxs-lookup"><span data-stu-id="15e93-209">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="15e93-210">Создайте файл *wwwroot/swagger/ui/css/custom.css* со следующим кодом CSS для настройки заголовка страницы:</span><span class="sxs-lookup"><span data-stu-id="15e93-210">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="15e93-211">Сошлитесь на файл *custom.css* в файле *index.html*:</span><span class="sxs-lookup"><span data-stu-id="15e93-211">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="15e93-212">Перейдите на страницу *index.html* в `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="15e93-212">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="15e93-213">Введите `http://localhost:<random_port>/swagger/v1/swagger.json` в текстовое поле заголовка и нажмите кнопку **Проводник**.</span><span class="sxs-lookup"><span data-stu-id="15e93-213">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="15e93-214">Полученная в итоге страница будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="15e93-214">The resulting page looks as follows:</span></span>

![Пользовательский интерфейс Swagger с настроенным заголовком](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="15e93-216">С этой страницей можно сделать гораздо больше.</span><span class="sxs-lookup"><span data-stu-id="15e93-216">There is much more you can do with the page.</span></span> <span data-ttu-id="15e93-217">Все возможности ресурсов пользовательского интерфейса см. в статье о [репозитории GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="15e93-217">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
