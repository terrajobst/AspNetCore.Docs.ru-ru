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
ms.openlocfilehash: fd2f415947c049d1239ce4e6bf0b1cf0264e7836
ms.sourcegitcommit: 41e3e007512c175a42910bc69678f3f0403cab04
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/01/2017
---
# <a name="aspnet-web-api-help-pages-using-swagger"></a><span data-ttu-id="b026e-104">Страницы справки по веб-API ASP.NET Core с использованием Swagger</span><span class="sxs-lookup"><span data-stu-id="b026e-104">ASP.NET Web API Help Pages using Swagger</span></span>

<a name=web-api-help-pages-using-swagger></a>

<span data-ttu-id="b026e-105">Авторы: [Шейн Бойер](https://twitter.com/spboyer) (Shayne Boyer) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="b026e-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="b026e-106">При сборке пользовательского приложения разработчик может обнаружить, что разобраться в различных методах API не так-то просто.</span><span class="sxs-lookup"><span data-stu-id="b026e-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="b026e-107">Используя [Swagger](http://swagger.io) с реализацией .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), можно получить хорошую документацию и справку по веб-API, добавив пару пакетов NuGet и скорректировав файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b026e-107">Generating good documentation and help pages for your Web API, using [Swagger](http://swagger.io) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="b026e-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) — это проект с открытым исходным кодом, предназначенный для создания документов Swagger по веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b026e-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="b026e-109">[Swagger](http://swagger.io) — это машиночитаемое представление API RESTful, которое обеспечивает поддержку интерактивной документации, создание клиентских пакетов SDK и возможность обнаружения.</span><span class="sxs-lookup"><span data-stu-id="b026e-109">[Swagger](http://swagger.io) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="b026e-110">В этом учебнике используется пример из статьи [Сборка первого веб-API с использованием ASP.NET Core MVC и Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="b026e-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="b026e-111">Если вы хотите повторить описанные действия, загрузите этот пример со страницы [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="b026e-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="b026e-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="b026e-112">Getting Started</span></span>

<span data-ttu-id="b026e-113">Swashbuckle включает три основных компонента.</span><span class="sxs-lookup"><span data-stu-id="b026e-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="b026e-114">`Swashbuckle.AspNetCore.Swagger`: объектная модель Swagger и ПО промежуточного слоя для предоставления объектов `SwaggerDocument` как конечных точек JSON.</span><span class="sxs-lookup"><span data-stu-id="b026e-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="b026e-115">`Swashbuckle.AspNetCore.SwaggerGen`: генератор Swagger, создающий объекты `SwaggerDocument` непосредственно из ваших маршрутов, контроллеров и моделей.</span><span class="sxs-lookup"><span data-stu-id="b026e-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="b026e-116">Как правило, он комбинируется с ПО промежуточного слоя конечной точки Swagger и за счет этого предоставляет Swagger JSON автоматически.</span><span class="sxs-lookup"><span data-stu-id="b026e-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="b026e-117">`Swashbuckle.AspNetCore.SwaggerUI`: встроенная версия пользовательского интерфейса Swagger, которая интерпретирует Swagger JSON и обеспечивает удобную, настраиваемую среду для описания функциональности веб-API.</span><span class="sxs-lookup"><span data-stu-id="b026e-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="b026e-118">Включает встроенные окружения тестов для открытых методов.</span><span class="sxs-lookup"><span data-stu-id="b026e-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="b026e-119">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="b026e-119">NuGet Packages</span></span>

<span data-ttu-id="b026e-120">Swashbuckle можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="b026e-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b026e-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b026e-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b026e-122">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="b026e-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="b026e-123">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="b026e-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="b026e-124">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b026e-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="b026e-125">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="b026e-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="b026e-126">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="b026e-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="b026e-127">Выберите пакет "Swashbuckle.AspNetCore" на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="b026e-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b026e-128">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="b026e-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b026e-129">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="b026e-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="b026e-130">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="b026e-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="b026e-131">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="b026e-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="b026e-132">В области результатов выберите пакет "Swashbuckle.AspNetCore" и нажмите кнопку **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="b026e-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b026e-133">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b026e-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b026e-134">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="b026e-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b026e-135">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b026e-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b026e-136">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b026e-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="b026e-137">Добавление и настройка Swagger для ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="b026e-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="b026e-138">Добавьте генератор Swagger в коллекцию служб в методе `ConfigureServices` файла *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b026e-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="b026e-139">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]</span><span class="sxs-lookup"><span data-stu-id="b026e-139">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]</span></span>

<span data-ttu-id="b026e-140">В методе `Configure` файла *Startup.cs* включите ПО промежуточного слоя для обслуживания созданного документа JSON и SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="b026e-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

<span data-ttu-id="b026e-141">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]</span><span class="sxs-lookup"><span data-stu-id="b026e-141">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]</span></span>

<span data-ttu-id="b026e-142">Запустите приложение и перейдите к файлу `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="b026e-142">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="b026e-143">Появится созданный документ с описанием конечных точек.</span><span class="sxs-lookup"><span data-stu-id="b026e-143">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="b026e-144">**Примечание**. Microsoft Edge, Google Chrome и Firefox отображают документы JSON изначально.</span><span class="sxs-lookup"><span data-stu-id="b026e-144">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="b026e-145">Существуют расширения для Chrome, которые форматируют документ для удобства чтения.</span><span class="sxs-lookup"><span data-stu-id="b026e-145">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="b026e-146">*Для краткости следующий пример приводится в сокращенном варианте.*</span><span class="sxs-lookup"><span data-stu-id="b026e-146">*The following example is reduced for brevity.*</span></span>

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

<span data-ttu-id="b026e-147">Этот документ запускает пользовательский интерфейс Swagger, для просмотра которого нужно перейти к `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="b026e-147">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Пользовательский интерфейс Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="b026e-149">Каждый метод открытого действия в `TodoController` можно протестировать в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="b026e-149">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="b026e-150">Щелкните имя метода, чтобы развернуть соответствующий раздел.</span><span class="sxs-lookup"><span data-stu-id="b026e-150">Click a method name to expand the section.</span></span> <span data-ttu-id="b026e-151">Добавьте все необходимые параметры и нажмите кнопку "Попробуйте!".</span><span class="sxs-lookup"><span data-stu-id="b026e-151">Add any necessary parameters, and click "Try it out!".</span></span>

![Пример тестирования в Swagger метода GET](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="b026e-153">Настройка и расширяемость</span><span class="sxs-lookup"><span data-stu-id="b026e-153">Customization & Extensibility</span></span>

<span data-ttu-id="b026e-154">Swagger предоставляет параметры для документирования объектной модели и настройки пользовательского интерфейса в соответствии с вашим фирменным стилем.</span><span class="sxs-lookup"><span data-stu-id="b026e-154">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="b026e-155">Данные и описание API</span><span class="sxs-lookup"><span data-stu-id="b026e-155">API Info and Description</span></span>

<span data-ttu-id="b026e-156">Действия по настройке, передаваемые в метод `AddSwaggerGen`, можно использовать для добавления таких сведений, как автор, лицензия и описание:</span><span class="sxs-lookup"><span data-stu-id="b026e-156">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

<span data-ttu-id="b026e-157">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]</span><span class="sxs-lookup"><span data-stu-id="b026e-157">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]</span></span>

<span data-ttu-id="b026e-158">Ниже показан фрагмент пользовательского интерфейса Swagger, в котором отображаются сведения о версии.</span><span class="sxs-lookup"><span data-stu-id="b026e-158">The following image depicts the Swagger UI displaying the version information:</span></span>

![Пользовательский интерфейс Swagger с данными версии: описание, автор и ссылка на дополнительные сведения](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="b026e-160">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="b026e-160">XML Comments</span></span>

<span data-ttu-id="b026e-161">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b026e-161">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b026e-162">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b026e-162">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b026e-163">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="b026e-163">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="b026e-164">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**:</span><span class="sxs-lookup"><span data-stu-id="b026e-164">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Свойства проекта, вкладка "Сборка"](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b026e-166">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="b026e-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b026e-167">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.</span><span class="sxs-lookup"><span data-stu-id="b026e-167">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="b026e-168">Установите флажок **Сформировать документацию XML** в разделе **Общие параметры**:</span><span class="sxs-lookup"><span data-stu-id="b026e-168">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Параметры проекта, раздел "Общие параметры"](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b026e-170">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b026e-170">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b026e-171">Вручную добавьте в файл *.csproj* следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="b026e-171">Manually add the following snippet to the *.csproj* file:</span></span>

<span data-ttu-id="b026e-172">[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]</span><span class="sxs-lookup"><span data-stu-id="b026e-172">[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]</span></span>

---

<span data-ttu-id="b026e-173">Настройте Swagger на использование созданного XML-файла.</span><span class="sxs-lookup"><span data-stu-id="b026e-173">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="b026e-174">В Linux и других операционных системах, отличных от Windows, имена и пути файлов могут быть чувствительны к регистру.</span><span class="sxs-lookup"><span data-stu-id="b026e-174">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="b026e-175">Например, файл *ToDoApi.XML* будет найден в Windows, но не в CentOS.</span><span class="sxs-lookup"><span data-stu-id="b026e-175">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

<span data-ttu-id="b026e-176">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]</span><span class="sxs-lookup"><span data-stu-id="b026e-176">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]</span></span>

<span data-ttu-id="b026e-177">В представленном выше коде `ApplicationBasePath` получает базовый путь приложения.</span><span class="sxs-lookup"><span data-stu-id="b026e-177">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="b026e-178">Базовый путь используется для поиска файла XML-комментариев.</span><span class="sxs-lookup"><span data-stu-id="b026e-178">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="b026e-179">*TodoApi.xml* работает только в этом примере, так как имя созданного файла комментариев XML основано на имени приложения.</span><span class="sxs-lookup"><span data-stu-id="b026e-179">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="b026e-180">Включение в метод комментариев с тройной косой чертой улучшает пользовательский интерфейс Swagger, так как позволяет добавить описание к заголовку раздела:</span><span class="sxs-lookup"><span data-stu-id="b026e-180">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

<span data-ttu-id="b026e-181">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="b026e-181">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]</span></span>

![Пользовательский интерфейс Swagger с XML-комментарием "Удаляет указанный элемент TodoItem"](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="b026e-184">Пользовательский интерфейс определяется созданным файлом JSON, который также содержит следующие комментарии.</span><span class="sxs-lookup"><span data-stu-id="b026e-184">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="b026e-185">Добавьте тег [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) в документацию к методу действия `Create`.</span><span class="sxs-lookup"><span data-stu-id="b026e-185">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="b026e-186">Он дополняет сведения, указанные в теге `<summary>`, и предоставляет более надежный пользовательский интерфейс Swagger.</span><span class="sxs-lookup"><span data-stu-id="b026e-186">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="b026e-187">Содержимое тега `<remarks>` может включать текст, JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="b026e-187">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

<span data-ttu-id="b026e-188">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="b026e-188">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>

<span data-ttu-id="b026e-189">Обратите внимание, как эти дополнительные комментарии улучшают пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="b026e-189">Notice the UI enhancements with these additional comments.</span></span>

![Пользовательский интерфейс Swagger с показанными дополнительными комментариями](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="b026e-191">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="b026e-191">Data Annotations</span></span>

<span data-ttu-id="b026e-192">Добавьте к контроллеру API атрибуты из `System.ComponentModel.DataAnnotations`, чтобы упростить реализацию компонентов пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="b026e-192">Decorate the API controller with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="b026e-193">Добавьте атрибут `[Required]` к свойству `Name` класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="b026e-193">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

<span data-ttu-id="b026e-194">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="b026e-194">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]</span></span>

<span data-ttu-id="b026e-195">Наличие этого атрибута изменяет поведение пользовательского интерфейса и схему базового JSON.</span><span class="sxs-lookup"><span data-stu-id="b026e-195">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="b026e-196">Добавьте к контроллеру API атрибут `[Produces("application/json")]`.</span><span class="sxs-lookup"><span data-stu-id="b026e-196">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="b026e-197">Он позволяет объявить, что действия контроллера поддерживают возврат типа содержимого *application/json*:</span><span class="sxs-lookup"><span data-stu-id="b026e-197">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

<span data-ttu-id="b026e-198">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="b026e-198">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]</span></span>

<span data-ttu-id="b026e-199">В раскрывающемся списке **Тип содержимого ответа** этот тип содержимого выбирается по умолчанию для операций контроллера GET.</span><span class="sxs-lookup"><span data-stu-id="b026e-199">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Пользовательский интерфейс Swagger с типом содержимого ответа по умолчанию](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="b026e-201">Чем больше заметок к данным используется в веб-API, тем более содержательными и полезными становятся пользовательский интерфейс и страницы справки по API.</span><span class="sxs-lookup"><span data-stu-id="b026e-201">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="b026e-202">Описания типов ответов</span><span class="sxs-lookup"><span data-stu-id="b026e-202">Describing Response Types</span></span>

<span data-ttu-id="b026e-203">Разработчиков пользовательских приложений в первую очередь волнуют возвращаемые данные &mdash; а именно типы ответов и коды ошибок (если они не стандартны).</span><span class="sxs-lookup"><span data-stu-id="b026e-203">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="b026e-204">Все это указывается в XML-комментарии и заметках к данным.</span><span class="sxs-lookup"><span data-stu-id="b026e-204">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="b026e-205">Действие `Create` возвращает `201 Created` в случае успеха и `400 Bad Request`, если текст опубликованного запроса пустой.</span><span class="sxs-lookup"><span data-stu-id="b026e-205">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="b026e-206">Без надлежащей документации в пользовательском интерфейсе Swagger пользователь не будет знать, чего ожидать.</span><span class="sxs-lookup"><span data-stu-id="b026e-206">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="b026e-207">Эту проблему решает добавление строк, выделенных в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b026e-207">That problem is fixed by adding the highlighted lines in the following example:</span></span>

<span data-ttu-id="b026e-208">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="b026e-208">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>

<span data-ttu-id="b026e-209">Теперь пользовательский интерфейс Swagger четко документирует ожидаемые коды HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="b026e-209">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Пользовательский интерфейс Swagger, показывающий описание класса ответа POST "Возвращает вновь созданный элемент Todo" и "400 — Если элемент имеет значение NULL" для кода состояния и причины в разделе "Сообщения ответов"](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="b026e-211">Настройка пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="b026e-211">Customizing the UI</span></span>

<span data-ttu-id="b026e-212">Стандартный пользовательский интерфейс функционален и представителен, но при сборке документации для своего API вы, скорее всего, захотите отобразить свой бренд или фирменный стиль.</span><span class="sxs-lookup"><span data-stu-id="b026e-212">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="b026e-213">Для решения этой задачи с использованием компонентов Swashbuckle необходимо добавить ресурсы для обслуживания статических файлов, а затем построить структуру папок для их размещения.</span><span class="sxs-lookup"><span data-stu-id="b026e-213">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="b026e-214">Если документация создается для платформы .NET Framework, добавьте в проект пакет NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="b026e-214">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="b026e-215">Включите ПО промежуточного слоя для статических файлов:</span><span class="sxs-lookup"><span data-stu-id="b026e-215">Enable the static files middleware:</span></span>

<span data-ttu-id="b026e-216">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="b026e-216">[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]</span></span>

<span data-ttu-id="b026e-217">Получите содержимое папки *dist* из [репозитория GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="b026e-217">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="b026e-218">Эта папка содержит ресурсы, необходимые для страниц пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="b026e-218">This folder contains the necessary assets for the Swagger UI page.</span></span> <span data-ttu-id="b026e-219">Скопируйте содержимое этой папки в папку *wwwroot/swagger/ui*.</span><span class="sxs-lookup"><span data-stu-id="b026e-219">Copy the contents of that folder into the *wwwroot/swagger/ui* folder.</span></span>

<span data-ttu-id="b026e-220">Создайте файл *wwwroot/swagger/ui/css/custom.css* со следующим кодом CSS для настройки заголовка страницы:</span><span class="sxs-lookup"><span data-stu-id="b026e-220">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

<span data-ttu-id="b026e-221">[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]</span><span class="sxs-lookup"><span data-stu-id="b026e-221">[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]</span></span>

<span data-ttu-id="b026e-222">Сошлитесь на файл *custom.css* в файле *index.html*:</span><span class="sxs-lookup"><span data-stu-id="b026e-222">Reference *custom.css* in the *index.html* file:</span></span>

<span data-ttu-id="b026e-223">[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]</span><span class="sxs-lookup"><span data-stu-id="b026e-223">[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]</span></span>

<span data-ttu-id="b026e-224">Перейдите на страницу *index.html* в `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="b026e-224">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="b026e-225">Введите `http://localhost:<random_port>/swagger/v1/swagger.json` в текстовое поле заголовка и нажмите кнопку **Проводник**.</span><span class="sxs-lookup"><span data-stu-id="b026e-225">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="b026e-226">Полученная в итоге страница будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b026e-226">The resulting page looks as follows:</span></span>

![Пользовательский интерфейс Swagger с настроенным заголовком](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="b026e-228">С этой страницей можно сделать гораздо больше.</span><span class="sxs-lookup"><span data-stu-id="b026e-228">There is much more you can do with the page.</span></span> <span data-ttu-id="b026e-229">Все возможности ресурсов пользовательского интерфейса см. в статье о [репозитории GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="b026e-229">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>