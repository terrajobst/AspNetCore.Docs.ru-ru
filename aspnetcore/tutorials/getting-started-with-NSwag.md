---
title: Начало работы с NSwag и ASP.NET Core
author: zuckerthoben
description: Узнайте, как использовать NSwag для создания документации и страниц справки для веб-API в ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: f4cc9ec1f32ef2bd0056ba8d0cbbbe9228834d85
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279208"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="56d9c-103">Начало работы с NSwag и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56d9c-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="56d9c-104">Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="56d9c-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="56d9c-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56d9c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="56d9c-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56d9c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="56d9c-107">Для использования [NSwag](https://github.com/RSuter/NSwag) с ПО промежуточного слоя ASP.NET Core требуется пакет NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="56d9c-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="56d9c-108">Пакет состоит из генератора Swagger, пользовательского интерфейса Swagger (v2 и v3) и [пользовательского интерфейса ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="56d9c-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="56d9c-109">Настоятельно рекомендуется использовать возможности создания кода в NSwag.</span><span class="sxs-lookup"><span data-stu-id="56d9c-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="56d9c-110">Выберите один из следующих вариантов создания кода:</span><span class="sxs-lookup"><span data-stu-id="56d9c-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="56d9c-111">Используйте [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), классическое приложение Windows для создания клиентского кода в C# и TypeScript для вашего API.</span><span class="sxs-lookup"><span data-stu-id="56d9c-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="56d9c-112">Используйте пакеты NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) или [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), чтобы создавать код внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="56d9c-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="56d9c-113">Используйте NSwag из [командной строки](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="56d9c-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="56d9c-114">Используйте пакет NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="56d9c-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="56d9c-115">Функции</span><span class="sxs-lookup"><span data-stu-id="56d9c-115">Features</span></span>

<span data-ttu-id="56d9c-116">Основная причина применения NSwag — возможность не только ввести пользовательский интерфейс Swagger и генератор Swagger, но и использовать гибкие возможности создания кода.</span><span class="sxs-lookup"><span data-stu-id="56d9c-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="56d9c-117">Вам не требуется существующий API &mdash; вы можете использовать API сторонних разработчиков, которые содержат Swagger и позволяют NSwag создавать реализацию клиента.</span><span class="sxs-lookup"><span data-stu-id="56d9c-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="56d9c-118">В любом случае цикл разработки ускоряется, и вам проще адаптироваться к изменениям API.</span><span class="sxs-lookup"><span data-stu-id="56d9c-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="56d9c-119">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="56d9c-119">Package installation</span></span>

<span data-ttu-id="56d9c-120">Пакет NuGet NSwag можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="56d9c-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56d9c-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56d9c-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="56d9c-122">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="56d9c-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="56d9c-123">Перейдите в раздел **Представление** > **Другие окна** > **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="56d9c-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="56d9c-124">Перейдите в каталог, в котором существует файл *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="56d9c-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="56d9c-125">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="56d9c-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="56d9c-126">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="56d9c-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="56d9c-127">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="56d9c-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="56d9c-128">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="56d9c-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="56d9c-129">В поле поиска введите "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="56d9c-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="56d9c-130">Выберите пакет "NSwag.AspNetCore" на вкладке **Обзор** и нажмите **Установить**</span><span class="sxs-lookup"><span data-stu-id="56d9c-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="56d9c-131">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="56d9c-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="56d9c-132">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="56d9c-133">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="56d9c-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="56d9c-134">В поле поиска введите "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="56d9c-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="56d9c-135">В области результатов выберите пакет "NSwag.AspNetCore" и нажмите **Добавить пакет**</span><span class="sxs-lookup"><span data-stu-id="56d9c-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="56d9c-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56d9c-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="56d9c-137">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="56d9c-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56d9c-138">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="56d9c-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="56d9c-139">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="56d9c-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="56d9c-140">Добавление и настройка ПО промежуточного слоя Swagger</span><span class="sxs-lookup"><span data-stu-id="56d9c-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="56d9c-141">Импортируйте следующие пространства имен в класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="56d9c-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="56d9c-142">В методе `Startup.Configure` включите ПО промежуточного слоя для обслуживания созданной спецификации Swagger и пользовательского интерфейса Swagger:</span><span class="sxs-lookup"><span data-stu-id="56d9c-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="56d9c-143">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="56d9c-143">Launch the app.</span></span> <span data-ttu-id="56d9c-144">Перейдите к `http://localhost:<port>/swagger` для просмотра пользовательского интерфейса Swagger.</span><span class="sxs-lookup"><span data-stu-id="56d9c-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="56d9c-145">Перейдите к `http://localhost:<port>/swagger/v1/swagger.json` для просмотра спецификации Swagger.</span><span class="sxs-lookup"><span data-stu-id="56d9c-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="56d9c-146">Создание кода</span><span class="sxs-lookup"><span data-stu-id="56d9c-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="56d9c-147">Через NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="56d9c-147">Via NSwagStudio</span></span>

* <span data-ttu-id="56d9c-148">Установите NSwagStudio из официального [репозитория GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="56d9c-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="56d9c-149">Запустите NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="56d9c-149">Launch NSwagStudio.</span></span> <span data-ttu-id="56d9c-150">Введите URL-адрес файла *swagger.json* в текстовое поле **URL-адрес спецификации Swagger** и нажмите кнопку **Создать локальную копию**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="56d9c-151">Выберите тип выходных данных **Клиент CSharp**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="56d9c-152">Другие доступные варианты: **клиент TypeScript** и **контроллер веб-API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="56d9c-153">Использование контроллера веб-API по сути является обратным созданием.</span><span class="sxs-lookup"><span data-stu-id="56d9c-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="56d9c-154">Служба воссоздается по спецификации службы.</span><span class="sxs-lookup"><span data-stu-id="56d9c-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="56d9c-155">Нажмите кнопку **Создать выходные данные**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="56d9c-156">Будет создана полная клиентская реализация проекта *TodoApi.NSwag* на языке C#.</span><span class="sxs-lookup"><span data-stu-id="56d9c-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="56d9c-157">Откройте вкладку **Клиент CSharp** в разделе **Выходные данные**, чтобы просмотреть созданный клиентский код:</span><span class="sxs-lookup"><span data-stu-id="56d9c-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="56d9c-158">Клиентский код C# создается на основе параметров, определенных на вкладке **Параметры** вкладки **Клиент CSharp**. Измените параметры для выполнения таких задач, как переименование пространства имен по умолчанию и создание синхронного метода.</span><span class="sxs-lookup"><span data-stu-id="56d9c-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="56d9c-159">Скопируйте созданный код C# в файл в клиентском проекте (например, приложение [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="56d9c-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="56d9c-160">Начните использовать веб-API:</span><span class="sxs-lookup"><span data-stu-id="56d9c-160">Start consuming the web API:</span></span>

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
> <span data-ttu-id="56d9c-161">Вы можете вставить базовый URL-адрес и (или) HTTP-клиент в API-клиент.</span><span class="sxs-lookup"><span data-stu-id="56d9c-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="56d9c-162">Рекомендуется всегда [повторно использовать HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="56d9c-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="56d9c-163">Другие способы создания кода клиента</span><span class="sxs-lookup"><span data-stu-id="56d9c-163">Other ways to generate client code</span></span>

<span data-ttu-id="56d9c-164">Можно создать клиентский код другими способами, более подходящими для вашего рабочего процесса:</span><span class="sxs-lookup"><span data-stu-id="56d9c-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="56d9c-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="56d9c-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="56d9c-166">В коде</span><span class="sxs-lookup"><span data-stu-id="56d9c-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="56d9c-167">Шаблоны T4</span><span class="sxs-lookup"><span data-stu-id="56d9c-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="56d9c-168">Настройка</span><span class="sxs-lookup"><span data-stu-id="56d9c-168">Customize</span></span>

<span data-ttu-id="56d9c-169">Swagger предоставляет параметры для документирования объектной модели и упрощения использования веб-API.</span><span class="sxs-lookup"><span data-stu-id="56d9c-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="56d9c-170">Данные и описание API</span><span class="sxs-lookup"><span data-stu-id="56d9c-170">API info and description</span></span>

<span data-ttu-id="56d9c-171">В методе `Startup.Configure` действие по настройке, передаваемое в метод `UseSwagger`, можно использовать для добавления таких сведений, как автор, лицензия и описание:</span><span class="sxs-lookup"><span data-stu-id="56d9c-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="56d9c-172">Пользовательский интерфейс Swagger отображает сведения о версии:</span><span class="sxs-lookup"><span data-stu-id="56d9c-172">The Swagger UI displays the version's information:</span></span>

![Пользовательский интерфейс Swagger с информацией о версии](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="56d9c-174">XML-комментарии</span><span class="sxs-lookup"><span data-stu-id="56d9c-174">XML comments</span></span>

<span data-ttu-id="56d9c-175">XML-комментарии можно включить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="56d9c-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="56d9c-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56d9c-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="56d9c-177">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="56d9c-178">Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**</span><span class="sxs-lookup"><span data-stu-id="56d9c-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="56d9c-179">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="56d9c-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="56d9c-180">Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.</span><span class="sxs-lookup"><span data-stu-id="56d9c-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="56d9c-181">Установите флажок **Сформировать XML-документацию** в разделе **Общие параметры**</span><span class="sxs-lookup"><span data-stu-id="56d9c-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="56d9c-182">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56d9c-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="56d9c-183">Вручную добавьте в файл *.csproj* следующий фрагмент кода:</span><span class="sxs-lookup"><span data-stu-id="56d9c-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="56d9c-184">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="56d9c-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="56d9c-185">NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать значение типа [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="56d9c-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="56d9c-186">Следовательно, NSwag не удается определить, что делает ваше действие и что оно возвращает.</span><span class="sxs-lookup"><span data-stu-id="56d9c-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="56d9c-187">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="56d9c-187">Consider the following example:</span></span>

<span data-ttu-id="56d9c-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="56d9c-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="56d9c-189">Предыдущее действие возвращает `IActionResult`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) или [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="56d9c-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="56d9c-190">Заметки к данным используются, чтобы сообщить клиентам, какие коды состояния HTTP возвращает это действие.</span><span class="sxs-lookup"><span data-stu-id="56d9c-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="56d9c-191">Добавьте к действию следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="56d9c-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="56d9c-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="56d9c-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="56d9c-193">NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать значение типа [IActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="56d9c-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="56d9c-194">Следовательно, NSwag может вывести только тип возвращаемого значения, определенный `T`.</span><span class="sxs-lookup"><span data-stu-id="56d9c-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="56d9c-195">Другие типы возвращаемых значений в действии вывести невозможно.</span><span class="sxs-lookup"><span data-stu-id="56d9c-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="56d9c-196">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="56d9c-196">Consider the following example:</span></span>

<span data-ttu-id="56d9c-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="56d9c-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="56d9c-198">Предыдущее действие возвращает `ActionResult<T>`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="56d9c-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="56d9c-199">Поскольку контроллер дополнен атрибутом [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), ответ [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) также возможен.</span><span class="sxs-lookup"><span data-stu-id="56d9c-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="56d9c-200">Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="56d9c-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="56d9c-201">Заметки к данным используются, чтобы сообщить клиентам, какие коды состояния HTTP возвращает это действие.</span><span class="sxs-lookup"><span data-stu-id="56d9c-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="56d9c-202">Добавьте к действию следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="56d9c-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="56d9c-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="56d9c-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="56d9c-204">Генератор Swagger теперь может точно описать это действие, и созданные клиенты знают, что они получают при вызове конечной точки.</span><span class="sxs-lookup"><span data-stu-id="56d9c-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="56d9c-205">Настоятельно рекомендуется добавлять эти атрибуты ко всем действиям.</span><span class="sxs-lookup"><span data-stu-id="56d9c-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="56d9c-206">Рекомендации о том, какие ответы HTTP должны возвращать ваши действия API, см. в [спецификации RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="56d9c-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
