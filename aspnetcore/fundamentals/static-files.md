---
title: "Работа с файлами статического в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как для обслуживания и защитить статические файлы и настройки размещения веб-приложение ASP.NET Core поведением по промежуточного слоя статических файлов."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="0eda0-103">Работа с файлами статического в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0eda0-103">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="0eda0-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Скотт Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="0eda0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="0eda0-105">Статические файлы, например HTML, CSS, изображения и JavaScript, являются активы приложения ASP.NET Core предоставляет клиентам напрямую.</span><span class="sxs-lookup"><span data-stu-id="0eda0-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="0eda0-106">Некоторые Настройка не требуется, чтобы включить для обслуживания этих файлов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="0eda0-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0eda0-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="0eda0-108">Обрабатывать статические файлы</span><span class="sxs-lookup"><span data-stu-id="0eda0-108">Serve static files</span></span>

<span data-ttu-id="0eda0-109">Статические файлы хранятся в корневом каталоге проекта web.</span><span class="sxs-lookup"><span data-stu-id="0eda0-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="0eda0-110">Каталог по умолчанию —  *\<content_root > / wwwroot*, но его можно изменить через [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) метод.</span><span class="sxs-lookup"><span data-stu-id="0eda0-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="0eda0-111">В разделе [содержимое корневого](xref:fundamentals/index#content-root) и [корневого веб-каталога](xref:fundamentals/index#web-root) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="0eda0-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="0eda0-112">Приложения веб-узел должен быть в курсе содержимого корневого каталога.</span><span class="sxs-lookup"><span data-stu-id="0eda0-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0eda0-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0eda0-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0eda0-114">`WebHost.CreateDefaultBuilder` Метод задает содержимое корневого в текущем каталоге:</span><span class="sxs-lookup"><span data-stu-id="0eda0-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0eda0-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0eda0-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0eda0-116">Задайте корневой содержимого в текущем каталоге путем вызова [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) внутри `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="0eda0-117">Статические файлы будут доступны с помощью путь относительно корневого веб-каталога.</span><span class="sxs-lookup"><span data-stu-id="0eda0-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="0eda0-118">Например **веб-приложение** шаблон проекта содержит несколько папок в *wwwroot* папки:</span><span class="sxs-lookup"><span data-stu-id="0eda0-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="0eda0-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="0eda0-119">**wwwroot**</span></span>
  * <span data-ttu-id="0eda0-120">**css**</span><span class="sxs-lookup"><span data-stu-id="0eda0-120">**css**</span></span>
  * <span data-ttu-id="0eda0-121">**images**</span><span class="sxs-lookup"><span data-stu-id="0eda0-121">**images**</span></span>
  * <span data-ttu-id="0eda0-122">**js**</span><span class="sxs-lookup"><span data-stu-id="0eda0-122">**js**</span></span>

<span data-ttu-id="0eda0-123">Для доступа к файлу в формате URI *изображения* вложенной *http://\<server_address > /images/\<image_file_name >*.</span><span class="sxs-lookup"><span data-stu-id="0eda0-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="0eda0-124">Например *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="0eda0-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0eda0-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0eda0-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0eda0-126">Если для различных версий платформы .NET Framework, добавьте [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="0eda0-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="0eda0-127">Если код предназначен для .NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) включает этот пакет.</span><span class="sxs-lookup"><span data-stu-id="0eda0-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0eda0-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0eda0-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0eda0-129">Добавить [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="0eda0-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="0eda0-130">Настройка [по промежуточного слоя](xref:fundamentals/middleware) позволяющее обслуживанием статических файлов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-130">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="0eda0-131">Обрабатывать файлы внутри корневого веб-каталога</span><span class="sxs-lookup"><span data-stu-id="0eda0-131">Serve files inside of web root</span></span>

<span data-ttu-id="0eda0-132">Вызвать [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) метода в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="0eda0-133">Без параметров `UseStaticFiles` перегруженный метод помечает файлы в корневой веб как servable.</span><span class="sxs-lookup"><span data-stu-id="0eda0-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="0eda0-134">Следующие ссылки разметки *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="0eda0-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="0eda0-135">Обрабатывать файлы за пределами корневого веб-каталога</span><span class="sxs-lookup"><span data-stu-id="0eda0-135">Serve files outside of web root</span></span>

<span data-ttu-id="0eda0-136">Рассмотрим Иерархия каталогов, в которой статические файлы обслуживать находятся за пределами корневого веб.</span><span class="sxs-lookup"><span data-stu-id="0eda0-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="0eda0-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="0eda0-137">**wwwroot**</span></span>
  * <span data-ttu-id="0eda0-138">**css**</span><span class="sxs-lookup"><span data-stu-id="0eda0-138">**css**</span></span>
  * <span data-ttu-id="0eda0-139">**images**</span><span class="sxs-lookup"><span data-stu-id="0eda0-139">**images**</span></span>
  * <span data-ttu-id="0eda0-140">**js**</span><span class="sxs-lookup"><span data-stu-id="0eda0-140">**js**</span></span>
* <span data-ttu-id="0eda0-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="0eda0-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="0eda0-142">**images**</span><span class="sxs-lookup"><span data-stu-id="0eda0-142">**images**</span></span>
      * <span data-ttu-id="0eda0-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="0eda0-143">*banner1.svg*</span></span>

<span data-ttu-id="0eda0-144">Запрос можно получить доступ к *banner1.svg* файла путем настройки по промежуточного слоя статических файлов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0eda0-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="0eda0-145">В приведенном выше коде *MyStaticFiles* Иерархия каталогов выполняется через публично *StaticFiles* сегмент URI.</span><span class="sxs-lookup"><span data-stu-id="0eda0-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="0eda0-146">Запрос на *http://\<server_address > /StaticFiles/images/banner1.svg* служит *banner1.svg* файла.</span><span class="sxs-lookup"><span data-stu-id="0eda0-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="0eda0-147">Следующие ссылки разметки *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="0eda0-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="0eda0-148">Задать заголовки ответов HTTP</span><span class="sxs-lookup"><span data-stu-id="0eda0-148">Set HTTP response headers</span></span>

<span data-ttu-id="0eda0-149">Объект [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) объект может использоваться для задания заголовков HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="0eda0-150">Кроме настройки обслуживание статических файлов от корня веб-, следующий код задает `Cache-Control` заголовка:</span><span class="sxs-lookup"><span data-stu-id="0eda0-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="0eda0-151">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) существует метод [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) пакета.</span><span class="sxs-lookup"><span data-stu-id="0eda0-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="0eda0-152">Файлы были сделаны публично кэшируемый 10 минут (600 секунд):</span><span class="sxs-lookup"><span data-stu-id="0eda0-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Отображение заголовка Cache-Control заголовки ответа были добавлены](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="0eda0-154">Авторизация статических файлов</span><span class="sxs-lookup"><span data-stu-id="0eda0-154">Static file authorization</span></span>

<span data-ttu-id="0eda0-155">По промежуточного слоя статических файлов не предоставляет возможность проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="0eda0-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="0eda0-156">Все файлы, обслуживаемых, включая те, в разделе *wwwroot*, являются открытыми.</span><span class="sxs-lookup"><span data-stu-id="0eda0-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="0eda0-157">Чтобы обрабатывать файлы на основе авторизации:</span><span class="sxs-lookup"><span data-stu-id="0eda0-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="0eda0-158">Сохраните их за пределами *wwwroot* и любой каталог, доступный для по промежуточного слоя статических файлов **и**</span><span class="sxs-lookup"><span data-stu-id="0eda0-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="0eda0-159">Обслуживает их через метод действия, к которому применяется авторизации.</span><span class="sxs-lookup"><span data-stu-id="0eda0-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="0eda0-160">Вернуть [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) объекта:</span><span class="sxs-lookup"><span data-stu-id="0eda0-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="0eda0-161">Включение просмотра каталогов</span><span class="sxs-lookup"><span data-stu-id="0eda0-161">Enable directory browsing</span></span>

<span data-ttu-id="0eda0-162">Просмотр каталогов позволяет пользователям веб-приложения см. список каталогов и файлов в указанном каталоге.</span><span class="sxs-lookup"><span data-stu-id="0eda0-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="0eda0-163">Обзор каталогов отключен по умолчанию, по соображениям безопасности (см. [вопросы](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="0eda0-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="0eda0-164">Просмотр с помощью вызова каталогов включения [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) метод в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="0eda0-165">Добавить требуемые службы путем вызова [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) метод `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="0eda0-166">Предыдущий код позволяет просматривать каталог *wwwroot/images* папки с помощью URL-адрес *http://\<server_address > / MyImages*, со ссылками на всех файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="0eda0-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Просмотр каталогов](static-files/_static/dir-browse.png)

<span data-ttu-id="0eda0-168">В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.</span><span class="sxs-lookup"><span data-stu-id="0eda0-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="0eda0-169">Обратите внимание на два `UseStaticFiles` вызывает в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="0eda0-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="0eda0-170">Первый вызов включает обслуживанием статических файлов в *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="0eda0-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="0eda0-171">Второй вызов включает просмотр каталогов из *wwwroot/images* папки с помощью URL-адрес *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="0eda0-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="0eda0-172">Обслуживать документ по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0eda0-172">Serve a default document</span></span>

<span data-ttu-id="0eda0-173">Настройка домашней страницы по умолчанию служит посетители логических отправной точкой при посещении веб-узла.</span><span class="sxs-lookup"><span data-stu-id="0eda0-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="0eda0-174">Чтобы обслуживать страницы по умолчанию без полного указания имен URI пользователь, вызовите [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) метод `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="0eda0-175">`UseDefaultFiles`должен быть вызван перед `UseStaticFiles` для обслуживания файла по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0eda0-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="0eda0-176">`UseDefaultFiles`является перезаписи URL-адрес, который фактически не использовать файл.</span><span class="sxs-lookup"><span data-stu-id="0eda0-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="0eda0-177">Включение по промежуточного слоя статических файлов через `UseStaticFiles` для обслуживания файла.</span><span class="sxs-lookup"><span data-stu-id="0eda0-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="0eda0-178">С `UseDefaultFiles`, запросы для поиска папки:</span><span class="sxs-lookup"><span data-stu-id="0eda0-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="0eda0-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="0eda0-179">*default.htm*</span></span>
* <span data-ttu-id="0eda0-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="0eda0-180">*default.html*</span></span>
* <span data-ttu-id="0eda0-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="0eda0-181">*index.htm*</span></span>
* <span data-ttu-id="0eda0-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="0eda0-182">*index.html*</span></span>

<span data-ttu-id="0eda0-183">Первый файл найден в списке обслуживается, как будто полный URI запроса.</span><span class="sxs-lookup"><span data-stu-id="0eda0-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="0eda0-184">URL-адрес браузера продолжает отражают запрошенного URI.</span><span class="sxs-lookup"><span data-stu-id="0eda0-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="0eda0-185">Следующий код позволяет изменить имя файла по умолчанию для *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="0eda0-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="0eda0-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="0eda0-186">UseFileServer</span></span>

<span data-ttu-id="0eda0-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) объединяет функциональные возможности `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="0eda0-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="0eda0-188">Следующий пример кода позволяет обслуживанием статические файлы и файл по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0eda0-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="0eda0-189">Просмотр каталогов не включена.</span><span class="sxs-lookup"><span data-stu-id="0eda0-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="0eda0-190">Следующий код основан перегрузки без параметров, позволяя просмотр каталогов:</span><span class="sxs-lookup"><span data-stu-id="0eda0-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="0eda0-191">Примите во внимание следующие иерархии каталога.</span><span class="sxs-lookup"><span data-stu-id="0eda0-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="0eda0-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="0eda0-192">**wwwroot**</span></span>
  * <span data-ttu-id="0eda0-193">**css**</span><span class="sxs-lookup"><span data-stu-id="0eda0-193">**css**</span></span>
  * <span data-ttu-id="0eda0-194">**images**</span><span class="sxs-lookup"><span data-stu-id="0eda0-194">**images**</span></span>
  * <span data-ttu-id="0eda0-195">**js**</span><span class="sxs-lookup"><span data-stu-id="0eda0-195">**js**</span></span>
* <span data-ttu-id="0eda0-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="0eda0-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="0eda0-197">**images**</span><span class="sxs-lookup"><span data-stu-id="0eda0-197">**images**</span></span>
      * <span data-ttu-id="0eda0-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="0eda0-198">*banner1.svg*</span></span>
  * <span data-ttu-id="0eda0-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="0eda0-199">*default.html*</span></span>

<span data-ttu-id="0eda0-200">Следующий пример кода позволяет статические файлы, файлы по умолчанию и просмотр каталога из `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="0eda0-201">`AddDirectoryBrowser`должно вызываться при `EnableDirectoryBrowsing` значение свойства `true`:</span><span class="sxs-lookup"><span data-stu-id="0eda0-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="0eda0-202">С помощью иерархии файлов и предшествующий код, URL-адреса устранить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0eda0-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="0eda0-203">URI</span><span class="sxs-lookup"><span data-stu-id="0eda0-203">URI</span></span>            |                             <span data-ttu-id="0eda0-204">Ответ</span><span class="sxs-lookup"><span data-stu-id="0eda0-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="0eda0-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="0eda0-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="0eda0-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="0eda0-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="0eda0-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="0eda0-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="0eda0-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="0eda0-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="0eda0-209">Если нет файла с именем по умолчанию в *MyStaticFiles* каталога, *http://\<server_address > / StaticFiles* возвращает каталога с интерактивными ссылками:</span><span class="sxs-lookup"><span data-stu-id="0eda0-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Список статических файлов](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="0eda0-211">`UseDefaultFiles`и `UseDirectoryBrowser` URL-адрес *http://\<server_address > / StaticFiles* без завершающей косой черты для запуска на стороне клиента перенаправления *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="0eda0-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="0eda0-212">Обратите внимание на добавленную конечную косую черту.</span><span class="sxs-lookup"><span data-stu-id="0eda0-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="0eda0-213">Относительные URL-адреса в документах, считаются недопустимый без косую черту в конце.</span><span class="sxs-lookup"><span data-stu-id="0eda0-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="0eda0-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="0eda0-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="0eda0-215">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) класс содержит `Mappings` свойство, служащее сопоставление расширений файлов для типов содержимого MIME.</span><span class="sxs-lookup"><span data-stu-id="0eda0-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="0eda0-216">В следующем примере несколько расширений файлов, регистрируются в известные типы MIME.</span><span class="sxs-lookup"><span data-stu-id="0eda0-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="0eda0-217">*.Rtf* заменяется расширения, и *.mp4* удаляется.</span><span class="sxs-lookup"><span data-stu-id="0eda0-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="0eda0-218">В разделе [типы содержимого MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="0eda0-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="0eda0-219">Нестандартные типы содержимого</span><span class="sxs-lookup"><span data-stu-id="0eda0-219">Non-standard content types</span></span>

<span data-ttu-id="0eda0-220">По промежуточного слоя статических файлов, которые понимает почти 400 типы содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="0eda0-221">Если пользователь запрашивает файл неизвестного типа, по промежуточного слоя статических файлов возвращает ответ HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="0eda0-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="0eda0-222">Если просмотр каталогов включен, отображается ссылка на файл.</span><span class="sxs-lookup"><span data-stu-id="0eda0-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="0eda0-223">URI возвращает ошибку HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="0eda0-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="0eda0-224">Следующий код позволяет обслуживает неизвестные типы, а также преобразование Неизвестный файл как изображение:</span><span class="sxs-lookup"><span data-stu-id="0eda0-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="0eda0-225">В вышеописанном примере кода запрос файл неизвестного типа содержимого возвращается в виде изображения.</span><span class="sxs-lookup"><span data-stu-id="0eda0-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="0eda0-226">Включение [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="0eda0-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="0eda0-227">По умолчанию оно отключено, и его использование не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="0eda0-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="0eda0-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) предоставляет более безопасная альтернатива обслужить файлы с нестандартные расширения.</span><span class="sxs-lookup"><span data-stu-id="0eda0-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="0eda0-229">Особенности</span><span class="sxs-lookup"><span data-stu-id="0eda0-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="0eda0-230">`UseDirectoryBrowser`и `UseStaticFiles` может вызвать утечку секретные данные.</span><span class="sxs-lookup"><span data-stu-id="0eda0-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="0eda0-231">Настоятельно рекомендуется отключить просмотр в рабочей среде каталогов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="0eda0-232">Внимательно просмотрите, какие каталоги включаются посредством `UseStaticFiles` или `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="0eda0-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="0eda0-233">Весь каталог и его подкаталогах становятся доступен из Интернета.</span><span class="sxs-lookup"><span data-stu-id="0eda0-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="0eda0-234">Хранилище файлов подходит для выступают в роли Public в выделенный каталог, например  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0eda0-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="0eda0-235">Эти файлы отдельные представления MVC, страниц Razor (только 2.x), файлы конфигурации и т. д.</span><span class="sxs-lookup"><span data-stu-id="0eda0-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="0eda0-236">URL-адреса для содержимого, представлены `UseDirectoryBrowser` и `UseStaticFiles` чувствительность к регистру и ограничения на символы базовой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="0eda0-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="0eda0-237">Например, без учета регистра Windows&mdash;не Mac и Linux.</span><span class="sxs-lookup"><span data-stu-id="0eda0-237">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="0eda0-238">Приложения ASP.NET Core, размещенные в IIS, воспользуйтесь [модуля ASP.NET Core (ANCM)](xref:fundamentals/servers/aspnet-core-module) для перенаправления всех запросов приложений, включая запросы статических файлов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="0eda0-239">Обработчик файла статистики IIS не используется.</span><span class="sxs-lookup"><span data-stu-id="0eda0-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="0eda0-240">Он имеет не может обработать запросы, прежде чем вы обрабатываются ANCM.</span><span class="sxs-lookup"><span data-stu-id="0eda0-240">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="0eda0-241">Выполните следующие действия в диспетчере служб IIS для удаления обработчику файла статистики на уровне сервера или веб-сайт IIS.</span><span class="sxs-lookup"><span data-stu-id="0eda0-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="0eda0-242">Перейдите к **модули** компонентов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="0eda0-243">Выберите **StaticFileModule** в списке.</span><span class="sxs-lookup"><span data-stu-id="0eda0-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="0eda0-244">Нажмите кнопку **удалить** в **действия** боковой панели.</span><span class="sxs-lookup"><span data-stu-id="0eda0-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="0eda0-245">Если обработчик файла статистики IIS включена **и** ANCM настроен неправильно, обслуживаются статических файлов.</span><span class="sxs-lookup"><span data-stu-id="0eda0-245">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="0eda0-246">Это происходит, например, если *web.config* файл не развернут.</span><span class="sxs-lookup"><span data-stu-id="0eda0-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="0eda0-247">Поместите файлы кода (включая *.cs* и *.cshtml*) за пределами корневого веб-каталога проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="0eda0-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="0eda0-248">Логическое разделение таким образом создается между содержимым клиентского и серверного кода приложения.</span><span class="sxs-lookup"><span data-stu-id="0eda0-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="0eda0-249">Это предотвращает утечку серверный код.</span><span class="sxs-lookup"><span data-stu-id="0eda0-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0eda0-250">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0eda0-250">Additional resources</span></span>

* [<span data-ttu-id="0eda0-251">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="0eda0-251">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="0eda0-252">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0eda0-252">Introduction to ASP.NET Core</span></span>](xref:index)
