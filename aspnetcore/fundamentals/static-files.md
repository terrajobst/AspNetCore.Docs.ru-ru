---
title: "Работа с статических файлов в ASP.NET Core"
author: rick-anderson
description: "Дополнительные сведения о работе с статических файлов в ASP.NET Core."
keywords: "ASP.NET Core, статические файлы, статические активы, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40c9a799c6ac8a2ce712df4b8fbf3c142ef3fd82
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="7eebe-104">Работа с статических файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7eebe-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="7eebe-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7eebe-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7eebe-106">Статические файлы, такие как HTML, CSS, JavaScript и изображение, активы, которые может обслуживать приложения ASP.NET Core непосредственно на клиентах.</span><span class="sxs-lookup"><span data-stu-id="7eebe-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="7eebe-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7eebe-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="7eebe-108">Обработку статических файлов</span><span class="sxs-lookup"><span data-stu-id="7eebe-108">Serving static files</span></span>

<span data-ttu-id="7eebe-109">Статические файлы обычно хранятся в `web root` (*\<содержимое корневой > / wwwroot*) папки.</span><span class="sxs-lookup"><span data-stu-id="7eebe-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="7eebe-110">В разделе [содержимое корневого](xref:fundamentals/index#content-root) и [корневого веб-каталога](xref:fundamentals/index#web-root) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="7eebe-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="7eebe-111">Обычно задать содержимое корневого для текущего каталога, чтобы ваш проект `web root` будет найден во время разработки.</span><span class="sxs-lookup"><span data-stu-id="7eebe-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="7eebe-112">Статические файлы могут храниться в любой папке под `web root` и доступ по относительному пути, корневой каталог.</span><span class="sxs-lookup"><span data-stu-id="7eebe-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="7eebe-113">Например, при создании проект веб-приложения по умолчанию, с помощью Visual Studio существует несколько папок, созданных в *wwwroot* папку - *css*, *изображения*, и *js*.</span><span class="sxs-lookup"><span data-stu-id="7eebe-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="7eebe-114">URI для доступа к изображению в *изображения* вложенную папку:</span><span class="sxs-lookup"><span data-stu-id="7eebe-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="7eebe-115">Чтобы статические файлы к обработке, необходимо настроить [по промежуточного слоя](middleware.md) добавление статических файлов в конвейер.</span><span class="sxs-lookup"><span data-stu-id="7eebe-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="7eebe-116">По промежуточного слоя статических файлов можно настроить путем добавления зависимость на *Microsoft.AspNetCore.StaticFiles* пакета в проект и затем вызова `UseStaticFiles` метод расширения из `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="7eebe-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="7eebe-117">`app.UseStaticFiles();`устанавливает файлы в `web root` (*wwwroot* по умолчанию) servable.</span><span class="sxs-lookup"><span data-stu-id="7eebe-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="7eebe-118">Далее будет показано как сделать servable с другими содержимое каталога `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7eebe-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="7eebe-119">Необходимо включить в пакет NuGet «Microsoft.AspNetCore.StaticFiles».</span><span class="sxs-lookup"><span data-stu-id="7eebe-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="7eebe-120">`web root`по умолчанию используется значение *wwwroot* каталога, но можно задать `web root` каталог с `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="7eebe-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="7eebe-121">Предположим, что имеется иерархии проекта, где находятся статические файлы, которые вы хотите использовать за пределами `web root`.</span><span class="sxs-lookup"><span data-stu-id="7eebe-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="7eebe-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="7eebe-122">For example:</span></span>

* <span data-ttu-id="7eebe-123">wwwroot</span><span class="sxs-lookup"><span data-stu-id="7eebe-123">wwwroot</span></span>
  * <span data-ttu-id="7eebe-124">css</span><span class="sxs-lookup"><span data-stu-id="7eebe-124">css</span></span>
  * <span data-ttu-id="7eebe-125">images</span><span class="sxs-lookup"><span data-stu-id="7eebe-125">images</span></span>
  * <span data-ttu-id="7eebe-126">...</span><span class="sxs-lookup"><span data-stu-id="7eebe-126">...</span></span>
* <span data-ttu-id="7eebe-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="7eebe-127">MyStaticFiles</span></span>
  * <span data-ttu-id="7eebe-128">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="7eebe-128">test.png</span></span>

<span data-ttu-id="7eebe-129">Для запроса на доступ к *test.png*, настройте по промежуточного слоя статических файлов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7eebe-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="7eebe-130">Запрос на `http://<app>/StaticFiles/test.png` будет обслуживать *test.png* файла.</span><span class="sxs-lookup"><span data-stu-id="7eebe-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="7eebe-131">`StaticFileOptions()`можно задать заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="7eebe-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="7eebe-132">Например, приведенный ниже код задает обработку из статических файлов *wwwroot* папок и наборов `Cache-Control` заголовок, чтобы сделать их публично кэшируемый 10 минут (600 секунд):</span><span class="sxs-lookup"><span data-stu-id="7eebe-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Отображение заголовка Cache-Control заголовки ответа были добавлены](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="7eebe-134">Авторизация статических файлов</span><span class="sxs-lookup"><span data-stu-id="7eebe-134">Static file authorization</span></span>

<span data-ttu-id="7eebe-135">Предоставляет статический файл модуля **не** проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="7eebe-135">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="7eebe-136">Все файлы, обслуживаемых, включая те, в разделе *wwwroot* являются общедоступными.</span><span class="sxs-lookup"><span data-stu-id="7eebe-136">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="7eebe-137">Чтобы обрабатывать файлы на основе авторизации:</span><span class="sxs-lookup"><span data-stu-id="7eebe-137">To serve files based on authorization:</span></span>

* <span data-ttu-id="7eebe-138">Сохраните их за пределами *wwwroot* и любой каталог, доступный для по промежуточного слоя статических файлов **и**</span><span class="sxs-lookup"><span data-stu-id="7eebe-138">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="7eebe-139">Обслуживать их через действия контроллера, возвращая `FileResult` применении авторизации</span><span class="sxs-lookup"><span data-stu-id="7eebe-139">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="7eebe-140">Включение просмотра каталогов</span><span class="sxs-lookup"><span data-stu-id="7eebe-140">Enabling directory browsing</span></span>

<span data-ttu-id="7eebe-141">Просмотр каталогов позволяет пользователю просмотреть список каталогов и файлов в указанном каталоге веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="7eebe-141">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="7eebe-142">Обзор каталогов отключен по умолчанию, по соображениям безопасности (см. [вопросы](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="7eebe-142">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="7eebe-143">Чтобы включить просмотр каталогов, вызовите `UseDirectoryBrowser` метод расширения из `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="7eebe-143">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="7eebe-144">И добавить требуемые службы путем вызова `AddDirectoryBrowser` метод расширения из `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7eebe-144">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="7eebe-145">Приведенный выше код позволяет просматривать каталог *wwwroot/images* папки с помощью URL-адрес http://\<приложения > / MyImages со ссылками на всех файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="7eebe-145">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![Просмотр каталогов](static-files/_static/dir-browse.png)

<span data-ttu-id="7eebe-147">В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.</span><span class="sxs-lookup"><span data-stu-id="7eebe-147">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="7eebe-148">Обратите внимание на два `app.UseStaticFiles` вызовов.</span><span class="sxs-lookup"><span data-stu-id="7eebe-148">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="7eebe-149">Первый необходимый для обслуживания CSS, изображения и JavaScript в *wwwroot* папки, а второй вызов для просмотра каталога *wwwroot/images* папки с помощью URL-адрес http://\<приложения > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="7eebe-149">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="7eebe-150">Обслуживает документ по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7eebe-150">Serving a default document</span></span>

<span data-ttu-id="7eebe-151">Установка домашней страницы по умолчанию предоставляет посетители сайта можно запустить при посещении веб-узла.</span><span class="sxs-lookup"><span data-stu-id="7eebe-151">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="7eebe-152">Для веб-приложения для обслуживания страницы по умолчанию без участия пользователя полные URI, вызывать `UseDefaultFiles` метод расширения из `Startup.Configure` следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7eebe-152">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="7eebe-153">`UseDefaultFiles`должен быть вызван перед `UseStaticFiles` для обслуживания файла по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7eebe-153">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="7eebe-154">`UseDefaultFiles`является повторной записи URL-адрес, который фактически не предоставлять этот файл.</span><span class="sxs-lookup"><span data-stu-id="7eebe-154">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="7eebe-155">Необходимо включить по промежуточного слоя статических файлов (`UseStaticFiles`) для обслуживания файла.</span><span class="sxs-lookup"><span data-stu-id="7eebe-155">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="7eebe-156">С `UseDefaultFiles`, будет выполнен поиск запросы в папку:</span><span class="sxs-lookup"><span data-stu-id="7eebe-156">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="7eebe-157">default.htm</span><span class="sxs-lookup"><span data-stu-id="7eebe-157">default.htm</span></span>
* <span data-ttu-id="7eebe-158">default.html</span><span class="sxs-lookup"><span data-stu-id="7eebe-158">default.html</span></span>
* <span data-ttu-id="7eebe-159">index.htm</span><span class="sxs-lookup"><span data-stu-id="7eebe-159">index.htm</span></span>
* <span data-ttu-id="7eebe-160">index.html</span><span class="sxs-lookup"><span data-stu-id="7eebe-160">index.html</span></span>

<span data-ttu-id="7eebe-161">Первый файл найден в списке будет предоставляться как в случае запроса полный URI (несмотря на то, что URL-адрес браузера будут отображать URI, запрошенный).</span><span class="sxs-lookup"><span data-stu-id="7eebe-161">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="7eebe-162">Ниже показано, как изменить имя файла по умолчанию для *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="7eebe-162">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="7eebe-163">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="7eebe-163">UseFileServer</span></span>

<span data-ttu-id="7eebe-164">`UseFileServer`объединяет функциональные возможности `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="7eebe-164">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="7eebe-165">Следующий пример кода позволяет статические файлы и файла для обслуживания, по умолчанию, но не допускает просмотр каталогов:</span><span class="sxs-lookup"><span data-stu-id="7eebe-165">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="7eebe-166">Следующий пример кода позволяет статических файлов по умолчанию и просмотр каталогов:</span><span class="sxs-lookup"><span data-stu-id="7eebe-166">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="7eebe-167">В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.</span><span class="sxs-lookup"><span data-stu-id="7eebe-167">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="7eebe-168">Как и в `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`, если вы хотите предоставлять файлы, которые существуют за пределами `web root`, создать и настроить `FileServerOptions` объекта, передаваемого как параметр `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="7eebe-168">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="7eebe-169">Например имеется следующая иерархия каталогов в веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="7eebe-169">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="7eebe-170">wwwroot</span><span class="sxs-lookup"><span data-stu-id="7eebe-170">wwwroot</span></span>

  * <span data-ttu-id="7eebe-171">css</span><span class="sxs-lookup"><span data-stu-id="7eebe-171">css</span></span>

  * <span data-ttu-id="7eebe-172">images</span><span class="sxs-lookup"><span data-stu-id="7eebe-172">images</span></span>

  * <span data-ttu-id="7eebe-173">...</span><span class="sxs-lookup"><span data-stu-id="7eebe-173">...</span></span>

* <span data-ttu-id="7eebe-174">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="7eebe-174">MyStaticFiles</span></span>

  * <span data-ttu-id="7eebe-175">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="7eebe-175">test.png</span></span>

  * <span data-ttu-id="7eebe-176">default.html</span><span class="sxs-lookup"><span data-stu-id="7eebe-176">default.html</span></span>

<span data-ttu-id="7eebe-177">Использовать приведенный выше пример иерархии, может потребоваться включить статические файлы, файлы по умолчанию и поиске `MyStaticFiles` каталога.</span><span class="sxs-lookup"><span data-stu-id="7eebe-177">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="7eebe-178">В следующем фрагменте кода, осуществляются с помощью одного вызова `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="7eebe-178">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="7eebe-179">Если `enableDirectoryBrowsing` равно `true` требуется вызывать `AddDirectoryBrowser` метод расширения из `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7eebe-179">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="7eebe-180">С помощью иерархии файлов и приведенный выше код:</span><span class="sxs-lookup"><span data-stu-id="7eebe-180">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="7eebe-181">URI</span><span class="sxs-lookup"><span data-stu-id="7eebe-181">URI</span></span>            |                             <span data-ttu-id="7eebe-182">Ответ</span><span class="sxs-lookup"><span data-stu-id="7eebe-182">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="7eebe-183">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="7eebe-183">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="7eebe-184">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="7eebe-184">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="7eebe-185">Если нет значения по умолчанию с именем файлы находятся в *MyStaticFiles* каталог, http://\<приложения > / StaticFiles возвращает каталога с интерактивными ссылками:</span><span class="sxs-lookup"><span data-stu-id="7eebe-185">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Список статических файлов](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="7eebe-187">`UseDefaultFiles`и `UseDirectoryBrowser` будет иметь URL-адрес http://\<приложения > / StaticFiles без завершающей косой черты и причина стороны клиента перенаправления http://\<приложения > /StaticFiles/ (Добавление завершающей косой черты).</span><span class="sxs-lookup"><span data-stu-id="7eebe-187">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="7eebe-188">Без конечные косая черта относительные URL-адреса в документах будет неверным.</span><span class="sxs-lookup"><span data-stu-id="7eebe-188">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="7eebe-189">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="7eebe-189">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="7eebe-190">`FileExtensionContentTypeProvider` Класс содержит коллекцию, которая сопоставляет расширения файлов для типов содержимого MIME.</span><span class="sxs-lookup"><span data-stu-id="7eebe-190">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="7eebe-191">В следующем образце зарегистрированных несколько расширений файлов для известных типов MIME, заменяется «.rtf» и «.mp4» удаляется.</span><span class="sxs-lookup"><span data-stu-id="7eebe-191">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="7eebe-192">В разделе [типы содержимого MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="7eebe-192">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="7eebe-193">Нестандартные типы содержимого</span><span class="sxs-lookup"><span data-stu-id="7eebe-193">Non-standard content types</span></span>

<span data-ttu-id="7eebe-194">По промежуточного слоя статических файлов ASP.NET понимает почти 400 типы содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="7eebe-194">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="7eebe-195">Если пользователь запрашивает файл неизвестного типа, по промежуточного слоя статических файлов возвращает ответ HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="7eebe-195">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="7eebe-196">Если просмотр каталогов включен, будет отображаться ссылка на файл, но URI будет возвращать ошибку HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="7eebe-196">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="7eebe-197">Следующий код позволяет обслуживает неизвестные типы и сделает Неизвестный файл как изображение.</span><span class="sxs-lookup"><span data-stu-id="7eebe-197">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="7eebe-198">С приведенный выше код запрашивает файл с Неизвестный тип содержимого будет возвращаться в виде изображения.</span><span class="sxs-lookup"><span data-stu-id="7eebe-198">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="7eebe-199">Включение `ServeUnknownFileTypes` представляет риск для безопасности и использовать его не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="7eebe-199">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="7eebe-200">`FileExtensionContentTypeProvider`(описано выше) предоставляет более безопасная альтернатива обслужить файлы с нестандартные расширения.</span><span class="sxs-lookup"><span data-stu-id="7eebe-200">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="7eebe-201">Особенности</span><span class="sxs-lookup"><span data-stu-id="7eebe-201">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="7eebe-202">`UseDirectoryBrowser`и `UseStaticFiles` может вызвать утечку секретные данные.</span><span class="sxs-lookup"><span data-stu-id="7eebe-202">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="7eebe-203">Рекомендуется, чтобы вы **не** directory Включение просмотра в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="7eebe-203">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="7eebe-204">Будьте внимательны о какие каталоги включения с `UseStaticFiles` или `UseDirectoryBrowser` как весь каталог и все вложенные каталоги будут доступны.</span><span class="sxs-lookup"><span data-stu-id="7eebe-204">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="7eebe-205">Рекомендуется оставлять открытый содержимое в своем собственном каталоге например  *\<содержимое корневого > / wwwroot*, от представления приложений, файлы конфигурации и т. д.</span><span class="sxs-lookup"><span data-stu-id="7eebe-205">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="7eebe-206">URL-адреса для содержимого, представлены `UseDirectoryBrowser` и `UseStaticFiles` чувствительность к регистру и ограничения на символы их базовой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="7eebe-206">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="7eebe-207">Например Windows не учитывает регистр, но Mac и Linux не.</span><span class="sxs-lookup"><span data-stu-id="7eebe-207">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="7eebe-208">Приложения ASP.NET Core, размещенные в IIS использовать модуль ASP.NET Core для перенаправления всех запросов приложения, включая запросы статических файлов.</span><span class="sxs-lookup"><span data-stu-id="7eebe-208">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="7eebe-209">Обработчик файла статистики IIS не используется, поскольку он не будет возможность обработки запросов, прежде чем они будут обработаны с модуль ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7eebe-209">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="7eebe-210">Чтобы удалить обработчик файла статистики IIS (на уровне сервера или веб-сайт):</span><span class="sxs-lookup"><span data-stu-id="7eebe-210">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="7eebe-211">Перейдите к **модули** функции</span><span class="sxs-lookup"><span data-stu-id="7eebe-211">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="7eebe-212">Выберите **StaticFileModule** в списке</span><span class="sxs-lookup"><span data-stu-id="7eebe-212">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="7eebe-213">Коснитесь **удалить** в **действия** боковой панели</span><span class="sxs-lookup"><span data-stu-id="7eebe-213">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="7eebe-214">Если обработчик файла статистики IIS включена **и** модуля ASP.NET Core (ANCM) настроен неправильно (например если *web.config* не было развернуто), невозможно предоставить статические файлы.</span><span class="sxs-lookup"><span data-stu-id="7eebe-214">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="7eebe-215">Файлы кода (включая c# и Razor) должны располагаться за пределами проекта приложения `web root` (*wwwroot* по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="7eebe-215">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="7eebe-216">При этом создается четкое разделение стороны содержимое своего приложения клиента и сервера стороны исходный код, который предотвращает утечку код со стороны сервера.</span><span class="sxs-lookup"><span data-stu-id="7eebe-216">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7eebe-217">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7eebe-217">Additional Resources</span></span>

* [<span data-ttu-id="7eebe-218">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="7eebe-218">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="7eebe-219">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7eebe-219">Introduction to ASP.NET Core</span></span>](../index.md)
