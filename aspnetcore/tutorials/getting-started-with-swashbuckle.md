---
title: Начало работы с Swashbuckle и ASP.NET Core
author: zuckerthoben
description: Дополнительные сведения о добавлении Swashbuckle в проект веб-API ASP.NET Core для интеграции пользовательского интерфейса Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/14/2018
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 9832e1ea2b59085b6680820469b16d549f4b0582
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635346"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="e2a87-103">Начало работы с Swashbuckle и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2a87-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="e2a87-104">Авторы: [Шейн Бойер](https://twitter.com/spboyer) (Shayne Boyer) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="e2a87-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e2a87-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2a87-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e2a87-106">Swashbuckle включает три основных компонента.</span><span class="sxs-lookup"><span data-stu-id="e2a87-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="e2a87-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): объектная модель Swagger и ПО промежуточного слоя для предоставления объектов `SwaggerDocument` как конечных точек JSON.</span><span class="sxs-lookup"><span data-stu-id="e2a87-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="e2a87-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): генератор Swagger, создающий объекты `SwaggerDocument` непосредственно из ваших маршрутов, контроллеров и моделей.</span><span class="sxs-lookup"><span data-stu-id="e2a87-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="e2a87-109">Как правило, он комбинируется с ПО промежуточного слоя конечной точки Swagger и за счет этого предоставляет Swagger JSON автоматически.</span><span class="sxs-lookup"><span data-stu-id="e2a87-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="e2a87-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): встроенная версия средства пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="e2a87-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="e2a87-111">Она интерпретирует Swagger JSON и обеспечивает удобную настраиваемую среду для описания функциональности веб-API.</span><span class="sxs-lookup"><span data-stu-id="e2a87-111">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="e2a87-112">Включает встроенные окружения тестов для открытых методов.</span><span class="sxs-lookup"><span data-stu-id="e2a87-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="e2a87-113">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="e2a87-113">Package installation</span></span>

<span data-ttu-id="e2a87-114">Swashbuckle можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="e2a87-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2a87-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2a87-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e2a87-116">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="e2a87-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="e2a87-117">Перейдите в раздел **Представление** > **Другие окна** > **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="e2a87-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="e2a87-118">Перейдите в каталог, в котором существует файл *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="e2a87-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="e2a87-119">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e2a87-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="e2a87-120">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="e2a87-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="e2a87-121">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="e2a87-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="e2a87-122">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="e2a87-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="e2a87-123">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="e2a87-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="e2a87-124">Выберите пакет "Swashbuckle.AspNetCore" на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e2a87-125">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e2a87-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e2a87-126">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="e2a87-127">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="e2a87-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="e2a87-128">В поле поиска введите "Swashbuckle.AspNetCore".</span><span class="sxs-lookup"><span data-stu-id="e2a87-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="e2a87-129">В области результатов выберите пакет Swashbuckle.AspNetCore и нажмите кнопку **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="e2a87-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e2a87-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2a87-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e2a87-131">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="e2a87-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2a87-132">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2a87-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e2a87-133">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e2a87-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="e2a87-134">Добавление и настройка ПО промежуточного слоя Swagger</span><span class="sxs-lookup"><span data-stu-id="e2a87-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="e2a87-135">Добавьте генератор Swagger в коллекцию служб в методе `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e2a87-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="e2a87-136">Импортируйте следующие пространства имен для использования в классе `Info`:</span><span class="sxs-lookup"><span data-stu-id="e2a87-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="e2a87-137">В методе `Startup.Configure` включите ПО промежуточного слоя для обслуживания созданного документа JSON и пользовательского интерфейса Swagger:</span><span class="sxs-lookup"><span data-stu-id="e2a87-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="e2a87-138">Предыдущий вызов метода `UseSwaggerUI` включает [ПО промежуточного слоя для статических файлов](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="e2a87-138">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="e2a87-139">Если код предназначен для .NET Framework или NET Core 1.x, добавьте в проект пакет NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/).</span><span class="sxs-lookup"><span data-stu-id="e2a87-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="e2a87-140">Запустите приложение и перейдите к файлу `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="e2a87-141">Появится созданный документ, описывающий конечные точки, как показано в [спецификации Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="e2a87-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="e2a87-142">Пользовательский интерфейс Swagger можно найти по адресу `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="e2a87-143">Изучите API через пользовательский интерфейс Swagger и включите его в другие программы.</span><span class="sxs-lookup"><span data-stu-id="e2a87-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="e2a87-144">Чтобы предоставить пользовательский интерфейс Swagger в корневом каталоге приложения (`http://localhost:<port>/`), установите для свойства `RoutePrefix` пустую строку:</span><span class="sxs-lookup"><span data-stu-id="e2a87-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="e2a87-145">Если вы используете виртуальные каталоги (например, в службах IIS или обратном прокси-сервере), задайте для конечной точки Swagger относительный путь с помощью префикса `./`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-145">If using virtual directories (with IIS or a reverse proxy, for example), set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="e2a87-146">Например, `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-146">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="e2a87-147">`/swagger/v1/swagger.json` указывает, что приложение должно искать JSON-файл в реальном корне URL-адреса (с префиксом маршрута, если он используется).</span><span class="sxs-lookup"><span data-stu-id="e2a87-147">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="e2a87-148">Например, `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` вместо `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-148">For example, `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="e2a87-149">Настройка и расширение</span><span class="sxs-lookup"><span data-stu-id="e2a87-149">Customize and extend</span></span>

<span data-ttu-id="e2a87-150">Swagger предоставляет параметры для документирования объектной модели и настройки пользовательского интерфейса в соответствии с вашим фирменным стилем.</span><span class="sxs-lookup"><span data-stu-id="e2a87-150">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="e2a87-151">Данные и описание API</span><span class="sxs-lookup"><span data-stu-id="e2a87-151">API info and description</span></span>

<span data-ttu-id="e2a87-152">Действие по настройке, передаваемое в метод `AddSwaggerGen`, можно использовать для добавления таких сведений, как автор, лицензия и описание:</span><span class="sxs-lookup"><span data-stu-id="e2a87-152">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="e2a87-153">Пользовательский интерфейс Swagger отображает сведения о версии:</span><span class="sxs-lookup"><span data-stu-id="e2a87-153">The Swagger UI displays the version's information:</span></span>

![Пользовательский интерфейс Swagger с данными версии: описание, автор и ссылка на дополнительные сведения](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="e2a87-155">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="e2a87-155">XML comments</span></span>

<span data-ttu-id="e2a87-156">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e2a87-156">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2a87-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2a87-157">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="e2a87-158">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите команду **Изменить <имя_проекта>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-158">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="e2a87-159">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e2a87-159">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="e2a87-160">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-160">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="e2a87-161">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e2a87-162">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e2a87-162">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="e2a87-163">В *панели решения* нажмите **Управление** и выберите имя проекта.</span><span class="sxs-lookup"><span data-stu-id="e2a87-163">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="e2a87-164">Перейдите в раздел **Сервис** > **Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-164">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="e2a87-165">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e2a87-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="e2a87-166">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**</span><span class="sxs-lookup"><span data-stu-id="e2a87-166">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="e2a87-167">Установите флажок **Сформировать XML-документацию** в разделе **Общие параметры**</span><span class="sxs-lookup"><span data-stu-id="e2a87-167">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e2a87-168">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2a87-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e2a87-169">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e2a87-169">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2a87-170">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2a87-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e2a87-171">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e2a87-171">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="e2a87-172">Включение комментариев XML предоставляет отладочную информацию для недокументированных открытых типов и членов.</span><span class="sxs-lookup"><span data-stu-id="e2a87-172">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="e2a87-173">Недокументированные типы и члены указываются в предупреждающем сообщении.</span><span class="sxs-lookup"><span data-stu-id="e2a87-173">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="e2a87-174">Например, следующее сообщение оповещает о нарушении кода предупреждения 1591:</span><span class="sxs-lookup"><span data-stu-id="e2a87-174">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="e2a87-175">Чтобы отключить предупреждения на уровне всего проекта, определите разделенный точками с запятой список игнорируемых кодов предупреждений в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="e2a87-175">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="e2a87-176">Добавление кодов предупреждений в `$(NoWarn);` также применяется к установленным по умолчанию значениям C#.</span><span class="sxs-lookup"><span data-stu-id="e2a87-176">Appending the warning codes to `$(NoWarn);` applies the C# default values too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="e2a87-177">Чтобы отключить предупреждения только для определенных элементов, поместите код в директивы препроцессора [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning).</span><span class="sxs-lookup"><span data-stu-id="e2a87-177">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="e2a87-178">Этот подход полезен, если код не должен быть доступен в документации API. В приведенном ниже примере код предупреждения CS1591 игнорируется для всего класса `Program`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-178">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="e2a87-179">Применение кода предупреждения можно восстановить в конце определения класса.</span><span class="sxs-lookup"><span data-stu-id="e2a87-179">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="e2a87-180">Укажите несколько кодов предупреждений в списке, разделив их запятыми.</span><span class="sxs-lookup"><span data-stu-id="e2a87-180">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="e2a87-181">Настройте Swagger на использование созданного XML-файла.</span><span class="sxs-lookup"><span data-stu-id="e2a87-181">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="e2a87-182">В Linux и других операционных системах, отличных от Windows, имена и пути файлов могут быть чувствительны к регистру.</span><span class="sxs-lookup"><span data-stu-id="e2a87-182">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="e2a87-183">Например, файл *ToDoApi.XML* допустим в Windows, но не в CentOS.</span><span class="sxs-lookup"><span data-stu-id="e2a87-183">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="e2a87-184">В приведенном выше коде для создания имени XML-файла, соответствующего имени проекта веб-API, используется [отражение](/dotnet/csharp/programming-guide/concepts/reflection).</span><span class="sxs-lookup"><span data-stu-id="e2a87-184">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="e2a87-185">Свойство [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) используется для построения пути к XML-файлу.</span><span class="sxs-lookup"><span data-stu-id="e2a87-185">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="e2a87-186">Включение в действие комментариев с тройной косой чертой улучшает пользовательский интерфейс Swagger, так как позволяет добавить описание к заголовку раздела.</span><span class="sxs-lookup"><span data-stu-id="e2a87-186">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="e2a87-187">Добавьте элемент [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) над действием `Delete`:</span><span class="sxs-lookup"><span data-stu-id="e2a87-187">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="e2a87-188">Пользовательский интерфейс Swagger отображает внутренний текст элемента `<summary>` в приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="e2a87-188">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Пользовательский интерфейс Swagger с XML-комментарием "Удаляет указанный элемент TodoItem"](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="e2a87-191">Пользовательский интерфейс определяется созданной схемой JSON:</span><span class="sxs-lookup"><span data-stu-id="e2a87-191">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="e2a87-192">Добавьте элемент [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) в документацию по методу действия `Create`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-192">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="e2a87-193">Он дополняет сведения, указанные в элементе `<summary>`, и предоставляет более надежный пользовательский интерфейс Swagger.</span><span class="sxs-lookup"><span data-stu-id="e2a87-193">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="e2a87-194">Содержимое элемента `<remarks>` может включать текст, JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="e2a87-194">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="e2a87-195">Обратите внимание, как эти дополнительные комментарии улучшают пользовательский интерфейс:</span><span class="sxs-lookup"><span data-stu-id="e2a87-195">Notice the UI enhancements with these additional comments:</span></span>

![Пользовательский интерфейс Swagger с показанными дополнительными комментариями](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="e2a87-197">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="e2a87-197">Data annotations</span></span>

<span data-ttu-id="e2a87-198">Дополните модель атрибутами из пространства имен [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations), чтобы обеспечить компоненты пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="e2a87-198">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="e2a87-199">Добавьте атрибут `[Required]` к свойству `Name` класса `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-199">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="e2a87-200">Наличие этого атрибута изменяет поведение пользовательского интерфейса и схему базового JSON.</span><span class="sxs-lookup"><span data-stu-id="e2a87-200">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="e2a87-201">Добавьте к контроллеру API атрибут `[Produces("application/json")]`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-201">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="e2a87-202">Он позволяет объявить, что действия контроллера поддерживают тип содержимого ответа *application/json*:</span><span class="sxs-lookup"><span data-stu-id="e2a87-202">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="e2a87-203">В раскрывающемся списке **Тип содержимого ответа** этот тип содержимого выбирается по умолчанию для операций контроллера GET.</span><span class="sxs-lookup"><span data-stu-id="e2a87-203">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Пользовательский интерфейс Swagger с типом содержимого ответа по умолчанию](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="e2a87-205">Чем больше заметок к данным используется в веб-API, тем более содержательными и полезными становятся пользовательский интерфейс и страницы справки по API.</span><span class="sxs-lookup"><span data-stu-id="e2a87-205">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="e2a87-206">Описание типов ответов</span><span class="sxs-lookup"><span data-stu-id="e2a87-206">Describe response types</span></span>

<span data-ttu-id="e2a87-207">Разработчиков пользовательских приложений в первую очередь волнуют возвращаемые данные &mdash; а именно: типы ответов и коды ошибок (если они не стандартны).</span><span class="sxs-lookup"><span data-stu-id="e2a87-207">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="e2a87-208">Типы ответов и коды ошибок обозначаются в комментариях к XML и заметках к данным.</span><span class="sxs-lookup"><span data-stu-id="e2a87-208">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="e2a87-209">Действие `Create` в случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="e2a87-209">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="e2a87-210">Код состояния HTTP 400 возвращается в том случае, когда текст отправленного запроса имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="e2a87-210">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="e2a87-211">Без надлежащей документации в пользовательском интерфейсе Swagger пользователь не будет знать, чего ожидать.</span><span class="sxs-lookup"><span data-stu-id="e2a87-211">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="e2a87-212">Эту проблему решает добавление строк, выделенных в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e2a87-212">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="e2a87-213">Теперь пользовательский интерфейс Swagger четко документирует ожидаемые коды HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="e2a87-213">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Пользовательский интерфейс Swagger, показывающий описание класса ответа POST "Возвращает вновь созданный элемент Todo" и "400 — Если элемент имеет значение NULL" для кода состояния и причины в разделе "Сообщения ответов"](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="e2a87-215">Настройка пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="e2a87-215">Customize the UI</span></span>

<span data-ttu-id="e2a87-216">Готовый пользовательский интерфейс отличается функциональностью и презентабельностью.</span><span class="sxs-lookup"><span data-stu-id="e2a87-216">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="e2a87-217">Но на страницах документации по API должна быть представлена ваша фирменная символика или тема.</span><span class="sxs-lookup"><span data-stu-id="e2a87-217">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="e2a87-218">Для оформления компонентов Swashbuckle в фирменном стиле необходимо добавить ресурсы для обслуживания статических файлов, а затем построить структуру папок для их размещения.</span><span class="sxs-lookup"><span data-stu-id="e2a87-218">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="e2a87-219">Если код предназначен для .NET Framework или NET Core 1.x, добавьте в проект пакет NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles):</span><span class="sxs-lookup"><span data-stu-id="e2a87-219">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="e2a87-220">Предыдущий пакет NuGet уже установлен, если код предназначен для .NET Core 2.x и используется [метапакет](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e2a87-220">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="e2a87-221">Включите ПО промежуточного слоя для статических файлов:</span><span class="sxs-lookup"><span data-stu-id="e2a87-221">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="e2a87-222">Получите содержимое папки *dist* из [репозитория GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="e2a87-222">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="e2a87-223">Эта папка содержит ресурсы, необходимые для страниц пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="e2a87-223">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="e2a87-224">Создайте папку *wwwroot/swagger/ui* и скопируйте в нее содержимое папки *dist*.</span><span class="sxs-lookup"><span data-stu-id="e2a87-224">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="e2a87-225">Создайте файл *custom.css* в *wwwroot/swagger/ui* со следующим CSS для настройки заголовка страницы:</span><span class="sxs-lookup"><span data-stu-id="e2a87-225">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="e2a87-226">Сошлитесь на файл *custom.css* в файле *index.html* после других файлов CSS:</span><span class="sxs-lookup"><span data-stu-id="e2a87-226">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="e2a87-227">Перейдите на страницу *index.html* в `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="e2a87-227">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="e2a87-228">Введите `http://localhost:<port>/swagger/v1/swagger.json` в текстовое поле заголовка и нажмите кнопку **Проводник**.</span><span class="sxs-lookup"><span data-stu-id="e2a87-228">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="e2a87-229">Полученная в итоге страница будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e2a87-229">The resulting page looks as follows:</span></span>

![Пользовательский интерфейс Swagger с настроенным заголовком](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="e2a87-231">С этой страницей можно сделать гораздо больше.</span><span class="sxs-lookup"><span data-stu-id="e2a87-231">There's much more you can do with the page.</span></span> <span data-ttu-id="e2a87-232">Все возможности ресурсов пользовательского интерфейса см. в статье о [репозитории GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="e2a87-232">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
