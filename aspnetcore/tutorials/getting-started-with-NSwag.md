---
title: Начало работы с NSwag и ASP.NET Core
author: zuckerthoben
description: Узнайте, как использовать NSwag для создания документации и страниц справки для веб-API в ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: b9266e2df75563be6bad1a1f464cef788c333d4c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028172"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="32b46-103">Начало работы с NSwag и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32b46-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="32b46-104">Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="32b46-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="32b46-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32b46-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="32b46-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32b46-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="32b46-107">Зарегистрируйте ПО промежуточного слоя NSwag, чтобы выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="32b46-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="32b46-108">создавать спецификации Swagger для реализованного веб-API;</span><span class="sxs-lookup"><span data-stu-id="32b46-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="32b46-109">применять пользовательский интерфейс Swagger для просмотра и тестирования веб-API.</span><span class="sxs-lookup"><span data-stu-id="32b46-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="32b46-110">Чтобы использовать ПО промежуточного слоя [NSwag](https://github.com/RSuter/NSwag) для ASP.NET Core, установите пакет NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="32b46-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="32b46-111">Этот пакет содержит ПО промежуточного слоя, которое позволяет создавать и использовать спецификацию Swagger, пользовательский интерфейс Swagger (версий 2 и 3) и [пользовательский интерфейс ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="32b46-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="32b46-112">Также настоятельно рекомендуется использовать возможности создания кода в NSwag.</span><span class="sxs-lookup"><span data-stu-id="32b46-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="32b46-113">Выберите один из следующих вариантов для использования возможностей создания кода:</span><span class="sxs-lookup"><span data-stu-id="32b46-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="32b46-114">Используйте [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), классическое приложение Windows для создания клиентского кода в C# и TypeScript для вашего API.</span><span class="sxs-lookup"><span data-stu-id="32b46-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="32b46-115">Используйте пакеты NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) или [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), чтобы создавать код внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="32b46-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="32b46-116">Используйте NSwag из [командной строки](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="32b46-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="32b46-117">Используйте пакет NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="32b46-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="32b46-118">Функции</span><span class="sxs-lookup"><span data-stu-id="32b46-118">Features</span></span>

<span data-ttu-id="32b46-119">Основная причина применения NSwag — возможность не только ввести пользовательский интерфейс Swagger и генератор Swagger, но и использовать гибкие возможности создания кода.</span><span class="sxs-lookup"><span data-stu-id="32b46-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="32b46-120">Вам не требуется существующий API &mdash; вы можете использовать API сторонних разработчиков, которые содержат Swagger и позволяют NSwag создавать реализацию клиента.</span><span class="sxs-lookup"><span data-stu-id="32b46-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="32b46-121">В любом случае цикл разработки ускоряется, и вам проще адаптироваться к изменениям API.</span><span class="sxs-lookup"><span data-stu-id="32b46-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="32b46-122">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="32b46-122">Package installation</span></span>

<span data-ttu-id="32b46-123">Пакет NuGet NSwag можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="32b46-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32b46-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32b46-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32b46-125">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="32b46-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="32b46-126">Перейдите в раздел **Представление** > **Другие окна** > **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="32b46-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="32b46-127">Перейдите в каталог, в котором существует файл *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="32b46-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="32b46-128">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="32b46-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="32b46-129">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="32b46-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="32b46-130">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="32b46-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="32b46-131">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="32b46-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="32b46-132">В поле поиска введите "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="32b46-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="32b46-133">Выберите пакет "NSwag.AspNetCore" на вкладке **Обзор** и нажмите **Установить**</span><span class="sxs-lookup"><span data-stu-id="32b46-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="32b46-134">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="32b46-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="32b46-135">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="32b46-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="32b46-136">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="32b46-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="32b46-137">В поле поиска введите "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="32b46-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="32b46-138">В области результатов выберите пакет "NSwag.AspNetCore" и нажмите **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="32b46-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="32b46-139">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="32b46-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="32b46-140">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="32b46-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32b46-141">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="32b46-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="32b46-142">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="32b46-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="32b46-143">Добавление и настройка ПО промежуточного слоя Swagger</span><span class="sxs-lookup"><span data-stu-id="32b46-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="32b46-144">Импортируйте следующие пространства имен в класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="32b46-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="32b46-145">Используя метод `Startup.ConfigureServices`, зарегистрируйте необходимые службы Swagger:</span><span class="sxs-lookup"><span data-stu-id="32b46-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="32b46-146">В методе `Startup.Configure` включите ПО промежуточного слоя для обслуживания созданной спецификации Swagger и пользовательского интерфейса Swagger версии 3:</span><span class="sxs-lookup"><span data-stu-id="32b46-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="32b46-147">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="32b46-147">Launch the app.</span></span> <span data-ttu-id="32b46-148">Перейдите к `http://localhost:<port>/swagger` для просмотра пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="32b46-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="32b46-149">Перейдите к `http://localhost:<port>/swagger/v1/swagger.json` для просмотра спецификации Swagger.</span><span class="sxs-lookup"><span data-stu-id="32b46-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="32b46-150">Создание кода</span><span class="sxs-lookup"><span data-stu-id="32b46-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="32b46-151">Через NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="32b46-151">Via NSwagStudio</span></span>

* <span data-ttu-id="32b46-152">Установите NSwagStudio из официального [репозитория GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="32b46-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="32b46-153">Запустите NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="32b46-153">Launch NSwagStudio.</span></span> <span data-ttu-id="32b46-154">Введите URL-адрес файла *swagger.json* в текстовое поле **URL-адрес спецификации Swagger** и нажмите кнопку **Создать локальную копию**.</span><span class="sxs-lookup"><span data-stu-id="32b46-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="32b46-155">Выберите тип выходных данных **Клиент CSharp**.</span><span class="sxs-lookup"><span data-stu-id="32b46-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="32b46-156">Другие доступные варианты: **клиент TypeScript** и **контроллер веб-API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="32b46-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="32b46-157">Использование контроллера веб-API по сути является обратным созданием.</span><span class="sxs-lookup"><span data-stu-id="32b46-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="32b46-158">Служба воссоздается по спецификации службы.</span><span class="sxs-lookup"><span data-stu-id="32b46-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="32b46-159">Нажмите кнопку **Создать выходные данные**.</span><span class="sxs-lookup"><span data-stu-id="32b46-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="32b46-160">Будет создана полная клиентская реализация проекта *TodoApi.NSwag* на языке C#.</span><span class="sxs-lookup"><span data-stu-id="32b46-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="32b46-161">Откройте вкладку **Клиент CSharp** в разделе **Выходные данные**, чтобы просмотреть созданный клиентский код:</span><span class="sxs-lookup"><span data-stu-id="32b46-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="32b46-162">Клиентский код C# создается на основе параметров, определенных на вкладке **Параметры** вкладки **Клиент CSharp**. Измените параметры для выполнения таких задач, как переименование пространства имен по умолчанию и создание синхронного метода.</span><span class="sxs-lookup"><span data-stu-id="32b46-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="32b46-163">Скопируйте созданный код C# в файл в клиентском проекте (например, приложение [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="32b46-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="32b46-164">Начните использовать веб-API:</span><span class="sxs-lookup"><span data-stu-id="32b46-164">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="32b46-165">Вы можете вставить базовый URL-адрес и (или) HTTP-клиент в API-клиент.</span><span class="sxs-lookup"><span data-stu-id="32b46-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="32b46-166">Рекомендуется всегда [повторно использовать HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="32b46-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="32b46-167">Другие способы создания кода клиента</span><span class="sxs-lookup"><span data-stu-id="32b46-167">Other ways to generate client code</span></span>

<span data-ttu-id="32b46-168">Можно создать клиентский код другими способами, более подходящими для вашего рабочего процесса:</span><span class="sxs-lookup"><span data-stu-id="32b46-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="32b46-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="32b46-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="32b46-170">В коде</span><span class="sxs-lookup"><span data-stu-id="32b46-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="32b46-171">Шаблоны T4</span><span class="sxs-lookup"><span data-stu-id="32b46-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="32b46-172">Настройка</span><span class="sxs-lookup"><span data-stu-id="32b46-172">Customize</span></span>

<span data-ttu-id="32b46-173">Swagger предоставляет параметры для документирования объектной модели и упрощения использования веб-API.</span><span class="sxs-lookup"><span data-stu-id="32b46-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="32b46-174">Данные и описание API</span><span class="sxs-lookup"><span data-stu-id="32b46-174">API info and description</span></span>

<span data-ttu-id="32b46-175">В методе `Startup.Configure` действие по настройке, передаваемое в метод `UseSwagger`, можно использовать для добавления таких сведений, как автор, лицензия и описание:</span><span class="sxs-lookup"><span data-stu-id="32b46-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="32b46-176">Пользовательский интерфейс Swagger отображает сведения о версии:</span><span class="sxs-lookup"><span data-stu-id="32b46-176">The Swagger UI displays the version's information:</span></span>

![Пользовательский интерфейс Swagger с информацией о версии](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="32b46-178">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="32b46-178">XML comments</span></span>

<span data-ttu-id="32b46-179">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="32b46-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="32b46-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32b46-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="32b46-181">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите команду **Изменить <имя_проекта>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="32b46-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="32b46-182">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="32b46-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="32b46-183">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="32b46-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="32b46-184">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**</span><span class="sxs-lookup"><span data-stu-id="32b46-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="32b46-185">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="32b46-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="32b46-186">В *панели решения* нажмите **Управление** и выберите имя проекта.</span><span class="sxs-lookup"><span data-stu-id="32b46-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="32b46-187">Перейдите в раздел **Сервис** > **Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="32b46-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="32b46-188">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="32b46-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="32b46-189">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**</span><span class="sxs-lookup"><span data-stu-id="32b46-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="32b46-190">Установите флажок **Сформировать XML-документацию** в разделе **Общие параметры**</span><span class="sxs-lookup"><span data-stu-id="32b46-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="32b46-191">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="32b46-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="32b46-192">Вручную добавьте выделенные строки в файл *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="32b46-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="32b46-193">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="32b46-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="32b46-194">NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать значение типа [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="32b46-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="32b46-195">Следовательно, NSwag не удается определить, что делает ваше действие и что оно возвращает.</span><span class="sxs-lookup"><span data-stu-id="32b46-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="32b46-196">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="32b46-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="32b46-197">Предыдущее действие возвращает `IActionResult`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) или [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="32b46-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="32b46-198">Заметки к данным используются, чтобы сообщить клиентам, какие коды состояния HTTP возвращает это действие.</span><span class="sxs-lookup"><span data-stu-id="32b46-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="32b46-199">Добавьте к действию следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="32b46-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="32b46-200">NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать значение типа [IActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="32b46-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="32b46-201">Следовательно, NSwag может вывести только тип возвращаемого значения, определенный `T`.</span><span class="sxs-lookup"><span data-stu-id="32b46-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="32b46-202">Другие типы возвращаемых значений в действии вывести невозможно.</span><span class="sxs-lookup"><span data-stu-id="32b46-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="32b46-203">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="32b46-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="32b46-204">Предыдущее действие возвращает `ActionResult<T>`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="32b46-204">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="32b46-205">Поскольку контроллер дополнен атрибутом [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), ответ [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) также возможен.</span><span class="sxs-lookup"><span data-stu-id="32b46-205">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="32b46-206">Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="32b46-206">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="32b46-207">Заметки к данным используются, чтобы сообщить клиентам, какие коды состояния HTTP возвращает это действие.</span><span class="sxs-lookup"><span data-stu-id="32b46-207">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="32b46-208">Добавьте к действию следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="32b46-208">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

<span data-ttu-id="32b46-209">Генератор Swagger теперь может точно описать это действие, и созданные клиенты знают, что они получают при вызове конечной точки.</span><span class="sxs-lookup"><span data-stu-id="32b46-209">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="32b46-210">Настоятельно рекомендуется добавлять эти атрибуты ко всем действиям.</span><span class="sxs-lookup"><span data-stu-id="32b46-210">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="32b46-211">Рекомендации о том, какие ответы HTTP должны возвращать ваши действия API, см. в [спецификации RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="32b46-211">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
