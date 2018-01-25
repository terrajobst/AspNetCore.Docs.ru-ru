---
title: "В ASP.NET Core передачи файлов"
author: ardalis
description: "Инструкции по использованию привязки модели и потоковой передачи для передачи файлов в ASP.NET Core MVC."
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: bc1cfe0d6ee88a0af49cdff9ce77ad42f57b95f7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="6ebd7-103">В ASP.NET Core передачи файлов</span><span class="sxs-lookup"><span data-stu-id="6ebd7-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="6ebd7-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6ebd7-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6ebd7-105">Действия ASP.NET MVC поддерживает возможность отправки одного или нескольких файлов, используя простую модель привязки для файлов меньшего размера или потоковая передача больших файлов.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="6ebd7-106">Просмотреть или загрузить пример из GitHub</span><span class="sxs-lookup"><span data-stu-id="6ebd7-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="6ebd7-107">Передача небольших файлов с использованием привязки модели</span><span class="sxs-lookup"><span data-stu-id="6ebd7-107">Uploading small files with model binding</span></span>

<span data-ttu-id="6ebd7-108">Чтобы отправить небольших файлов, можно использовать составной HTML-формы или создать запрос POST с использованием JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="6ebd7-109">Ниже приводится пример формы, с помощью Razor, поддерживающего несколько отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="6ebd7-110">Для поддержки передачи файлов, необходимо указать HTML-формы `enctype` из `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="6ebd7-111">`files` Входной элемент, показанный выше поддерживает передачу нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="6ebd7-112">Пропустить `multiple` атрибутов для этого входного элемента, чтобы разрешить только один файл для отправки.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="6ebd7-113">Выше Разметка отображает в браузере как:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-113">The above markup renders in a browser as:</span></span>

![Форму для загрузки файла](file-uploads/_static/upload-form.png)

<span data-ttu-id="6ebd7-115">Отдельные файлы, переданные на сервер можно получить с помощью [привязки модели](xref:mvc/models/model-binding) с помощью [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) интерфейса.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="6ebd7-116">`IFormFile`имеет следующую структуру:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="6ebd7-117">Не полагайтесь на или доверия `FileName` свойство без проверки.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="6ebd7-118">`FileName` Свойства должен использоваться только в целях отображения.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="6ebd7-119">При отправке файлов с использованием привязки модели и `IFormFile` интерфейс, метод действия может принимать один `IFormFile` или `IEnumerable<IFormFile>` (или `List<IFormFile>`) представляет несколько файлов.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="6ebd7-120">Следующий пример просматривает один или несколько переданных файлов, сохраняет их в локальной файловой системе и возвращает общее число и размер переданных файлов.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="6ebd7-121">Файлы загружаются с помощью `IFormFile` метод помещаются в буфер в памяти или на диске, на веб-сервере перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="6ebd7-122">Внутри метода действия `IFormFile` содержимое доступно в виде потока.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="6ebd7-123">В дополнение к локальной файловой системе, файлах может передаваться в [хранилища больших двоичных объектов Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) или [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="6ebd7-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="6ebd7-124">Для хранения двоичного файла данных в базе данных с использованием Entity Framework, определите свойство типа `byte[]` на сущность:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="6ebd7-125">Укажите свойства viewmodel типа `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="6ebd7-126">`IFormFile`можно использовать напрямую как параметр метода действия или свойства viewmodel, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="6ebd7-127">Копировать `IFormFile` в поток и сохраните его на массив байтов:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="6ebd7-128">Будьте внимательны при хранения двоичных данных в реляционных базах данных, как это может отрицательно повлиять на производительность.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="6ebd7-129">Передача больших файлов с помощью потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="6ebd7-129">Uploading large files with streaming</span></span>

<span data-ttu-id="6ebd7-130">Если размер или частоты передачи файлов вызывает проблемы ресурсов для приложения, рассмотрите возможность потоковой передачи передачу файла, а не используя буфер в полном объеме как показанный выше подход привязки модели.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="6ebd7-131">При использовании `IFormFile` и привязки модели — это гораздо проще решение, потоковой передачи требуется ряд действий для реализации должным образом.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="6ebd7-132">Любой файл одного буферизации, которых превышает 64 КБ будет перемещен из оперативной памяти во временный файл на диске на сервере.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="6ebd7-133">Ресурсы (диск ОЗУ), используемые передачи файлов зависят от числа и размера передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="6ebd7-134">Потоковая передача не столько производительности, о масштабе.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="6ebd7-135">При попытке буфер слишком много передачи веб-узла произойдет сбой в случае нехватки памяти или места на диске.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="6ebd7-136">В следующем примере показано использование JavaScript и угловой можно передавать на действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="6ebd7-137">Токен сложные файл создается с помощью пользовательского атрибута фильтра и передаются в HTTP-заголовки, а не в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="6ebd7-138">Так как метод действия обрабатывает отправляемые вами данные непосредственно, другой фильтр отключил привязки модели.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="6ebd7-139">В действии, содержимого формы считываются с использованием `MultipartReader`, который читает каждый отдельный `MultipartSection`, обработка файла или сохранение содержимого соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="6ebd7-140">После чтения всех разделов, действие выполняет собственную привязки модели.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="6ebd7-141">Начальное действие загружает форму и сохраняет токен сложные в файле cookie (через `GenerateAntiforgeryTokenCookieForAjax` атрибут):</span><span class="sxs-lookup"><span data-stu-id="6ebd7-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="6ebd7-142">Атрибут использует встроенные ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) поддержки, чтобы задать файл cookie с маркера запроса:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="6ebd7-143">Угловая автоматически передает сложные токена в заголовок запроса с именем `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="6ebd7-144">Приложение ASP.NET Core MVC настраивается для ссылки на этот заголовок в конфигурации в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="6ebd7-145">`DisableFormValueModelBinding` Атрибут, показанный ниже, используется для отключения привязки модели для `Upload` метода действия.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="6ebd7-146">После отключения привязки модели, `Upload` метод действия не принимают параметры.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="6ebd7-147">Он работает непосредственно с `Request` свойство `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="6ebd7-148">Объект `MultipartReader` используется для чтения в каждом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="6ebd7-149">Файл сохраняется с именем файла, GUID и хранения ключей и значений данных в `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="6ebd7-150">Как только все разделы были считаны, содержимое `KeyValueAccumulator` используются для привязки данных формы для типа модели.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="6ebd7-151">Полный `Upload` метода показан ниже:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="6ebd7-152">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="6ebd7-152">Troubleshooting</span></span>

<span data-ttu-id="6ebd7-153">Ниже приведены некоторые наиболее часто возникающие при работе с передача файлов и их возможные решения.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="6ebd7-154">Непредвиденная ошибка не найден в IIS</span><span class="sxs-lookup"><span data-stu-id="6ebd7-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="6ebd7-155">Следующая ошибка указывает на попытку отправки файла превышает сервера настроенного `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="6ebd7-156">Значение по умолчанию — `30000000`, который составляет примерно 28.6 МБ.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="6ebd7-157">Значение можно настроить, изменив *web.config*:</span><span class="sxs-lookup"><span data-stu-id="6ebd7-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="6ebd7-158">Этот параметр применяется только к службам IIS.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-158">This setting only applies to IIS.</span></span> <span data-ttu-id="6ebd7-159">Поведение не происходит по умолчанию при размещении на Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="6ebd7-160">Дополнительные сведения см. в разделе [ограничения запросов \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="6ebd7-160">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="6ebd7-161">Пустая ссылка на исключение IFormFile</span><span class="sxs-lookup"><span data-stu-id="6ebd7-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="6ebd7-162">Если ваш контроллер принимает передать файлы с помощью `IFormFile` , но вы можете найти значение всегда равно null, убедитесь, что формы HTML: необходимо указать `enctype` значение `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="6ebd7-163">Если этот атрибут не задан на `<form>` элемент, отправки файла не произойдет и любую границу `IFormFile` аргументы будут иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="6ebd7-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
