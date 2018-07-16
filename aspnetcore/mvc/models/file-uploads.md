---
title: Передача файлов в ASP.NET Core
author: ardalis
description: Сведения об использовании привязки модели и потоковой передачи для передачи файлов в ASP.NET Core MVC.
ms.author: riande
ms.date: 07/05/2017
uid: mvc/models/file-uploads
ms.openlocfilehash: 771e22ca01c67f2b6bbee780324d9d08759b3279
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38201736"
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="3f778-103">Передача файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f778-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="3f778-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="3f778-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3f778-105">Действия ASP.NET MVC поддерживают передачу одного или нескольких файлов с помощью простой привязки модели для небольших файлов или потоковой передачи для более крупных файлов.</span><span class="sxs-lookup"><span data-stu-id="3f778-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="3f778-106">Просмотреть или скачать образец с GitHub</span><span class="sxs-lookup"><span data-stu-id="3f778-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="3f778-107">Передача небольших файлов с помощью привязки модели</span><span class="sxs-lookup"><span data-stu-id="3f778-107">Uploading small files with model binding</span></span>

<span data-ttu-id="3f778-108">Для передачи небольших файлов можно применить составную HTML-форму или сформировать запрос POST на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3f778-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="3f778-109">Примеры формы, использующей синтаксис Razor и поддерживающей передачу нескольких файлов, приведен ниже.</span><span class="sxs-lookup"><span data-stu-id="3f778-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

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

<span data-ttu-id="3f778-110">Для поддержки передачи файлов в HTML-формах должен указываться атрибут `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3f778-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="3f778-111">Показанный выше элемент ввода `files` поддерживает передачу нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="3f778-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="3f778-112">Чтобы разрешить передачу только одного файла, пропустите атрибут `multiple` в этом элементе.</span><span class="sxs-lookup"><span data-stu-id="3f778-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="3f778-113">Приведенная выше разметка отрисовывается в браузере следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3f778-113">The above markup renders in a browser as:</span></span>

![Форма для отправки файла](file-uploads/_static/upload-form.png)

<span data-ttu-id="3f778-115">Доступ к отдельным файлам, переданным на сервер, можно получать посредством [привязки модели](xref:mvc/models/model-binding) с помощью интерфейса [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile).</span><span class="sxs-lookup"><span data-stu-id="3f778-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="3f778-116">`IFormFile` имеет следующую структуру:</span><span class="sxs-lookup"><span data-stu-id="3f778-116">`IFormFile` has the following structure:</span></span>

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
> <span data-ttu-id="3f778-117">Свойство `FileName` требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="3f778-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="3f778-118">Свойство `FileName` следует использовать только в целях отображения данных.</span><span class="sxs-lookup"><span data-stu-id="3f778-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="3f778-119">При передаче файлов с помощью привязки модели и интерфейса `IFormFile` метод действия может принимать либо одиночный интерфейс `IFormFile`, либо интерфейс `IEnumerable<IFormFile>` (или `List<IFormFile>`), представляющий несколько файлов.</span><span class="sxs-lookup"><span data-stu-id="3f778-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="3f778-120">В приведенном ниже примере осуществляется перебор одного или нескольких переданных файлов, после чего они сохраняются в локальной файловой системе, и возвращается количество и общий размер переданных файлов.</span><span class="sxs-lookup"><span data-stu-id="3f778-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="3f778-121">Файлы, передаваемые с помощью интерфейса `IFormFile`, буферизуются в памяти или на диске на веб-сервере перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="3f778-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="3f778-122">Внутри метода действия содержимое `IFormFile` доступно в виде потока.</span><span class="sxs-lookup"><span data-stu-id="3f778-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="3f778-123">Помимо локальной файловой системы, файлы могут передаваться в потоковом режиме в [хранилище BLOB-объектов Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) или в [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="3f778-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="3f778-124">Для сохранения данных двоичных файлов в базе данных с помощью Entity Framework определите для сущности свойство типа `byte[]`:</span><span class="sxs-lookup"><span data-stu-id="3f778-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="3f778-125">Определите свойство viewmodel типа `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="3f778-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="3f778-126">`IFormFile` можно использовать непосредственно как параметр метода действия или как свойство viewmodel, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="3f778-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="3f778-127">Скопируйте экземпляр `IFormFile` в поток и сохраните его в байтовом массиве:</span><span class="sxs-lookup"><span data-stu-id="3f778-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

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
> <span data-ttu-id="3f778-128">При сохранении двоичных данных в реляционных базах данных следует соблюдать осторожность, так как это может отрицательно сказаться на производительности.</span><span class="sxs-lookup"><span data-stu-id="3f778-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="3f778-129">Передача больших файлов с помощью потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="3f778-129">Uploading large files with streaming</span></span>

<span data-ttu-id="3f778-130">Если из-за размера передаваемых файлов или частоты их передачи в приложении возникают проблемы с ресурсами, рекомендуется применять потоковую передачу вместо буферизации файлов целиком, как в случае с описанным выше подходом на основе привязки модели.</span><span class="sxs-lookup"><span data-stu-id="3f778-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="3f778-131">Использовать интерфейс `IFormFile` и привязку модели гораздо проще, в то время как для правильной реализации потоковой передачи требуется ряд действий.</span><span class="sxs-lookup"><span data-stu-id="3f778-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="3f778-132">Если размер отдельного буферизуемого файла превышает 64 КБ, он перемещается из оперативной памяти во временный файл на диске на сервере.</span><span class="sxs-lookup"><span data-stu-id="3f778-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="3f778-133">Потребление ресурсов (диска, ОЗУ) при передаче файлов зависит от количества и размера одновременно передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="3f778-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="3f778-134">Потоковая передача призвана в первую очередь не повысить производительность, а обеспечить масштабирование.</span><span class="sxs-lookup"><span data-stu-id="3f778-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="3f778-135">При попытке поместить в буфер слишком много файлов может произойти аварийное завершение работы сайта из-за нехватки памяти или места на диске.</span><span class="sxs-lookup"><span data-stu-id="3f778-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="3f778-136">В приведенном ниже примере демонстрируется использование JavaScript и Angular для потоковой передачи в действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="3f778-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="3f778-137">Токен против подделки файла создается с помощью пользовательского атрибута фильтра и передается в заголовках HTTP, а не в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="3f778-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="3f778-138">Так как метод действия обрабатывает передаваемые данные напрямую, привязка модели отключается другим фильтром.</span><span class="sxs-lookup"><span data-stu-id="3f778-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="3f778-139">Внутри действия содержимое формы считывается с помощью объекта `MultipartReader`, который считывает каждый объект `MultipartSection` по отдельности, обрабатывая файл или сохраняя содержимое.</span><span class="sxs-lookup"><span data-stu-id="3f778-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="3f778-140">После считывания всех разделов действие выполняет собственную привязку модели.</span><span class="sxs-lookup"><span data-stu-id="3f778-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="3f778-141">Начальное действие загружает форму и сохраняет токен против подделки в файле cookie (с помощью атрибута `GenerateAntiforgeryTokenCookieForAjax`).</span><span class="sxs-lookup"><span data-stu-id="3f778-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="3f778-142">Атрибут использует встроенную поддержку [защиты от подделки](xref:security/anti-request-forgery) в ASP.NET Core, чтобы задать файл cookie с токеном запроса.</span><span class="sxs-lookup"><span data-stu-id="3f778-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="3f778-143">Angular автоматически передает токен от подделки в заголовке запроса с именем `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="3f778-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="3f778-144">Приложение ASP.NET Core MVC настроено так, что оно ссылается на этот заголовок в конфигурации в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f778-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="3f778-145">Показанный ниже атрибут `DisableFormValueModelBinding` предназначен для отключения привязки модели для метода действия `Upload`.</span><span class="sxs-lookup"><span data-stu-id="3f778-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="3f778-146">Так как привязка модели отключена, метод действия `Upload` не принимает параметров.</span><span class="sxs-lookup"><span data-stu-id="3f778-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="3f778-147">Он работает напрямую со свойством `Request` объекта `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="3f778-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="3f778-148">Для считывания каждого раздела служит объект `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="3f778-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="3f778-149">Файл сохраняется с именем GUID, а данные типа "ключ — значение" сохраняются в `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="3f778-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="3f778-150">После считывания всех разделов содержимое `KeyValueAccumulator` используется для привязки данных формы к типу модели.</span><span class="sxs-lookup"><span data-stu-id="3f778-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="3f778-151">Ниже представлен полный код метода `Upload`.</span><span class="sxs-lookup"><span data-stu-id="3f778-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="3f778-152">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="3f778-152">Troubleshooting</span></span>

<span data-ttu-id="3f778-153">Ниже описываются некоторые распространенные проблемы, которые возникают при передаче файлов, и возможные способы их решения.</span><span class="sxs-lookup"><span data-stu-id="3f778-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="3f778-154">Непредвиденная ошибка "Не найдено" в службах IIS</span><span class="sxs-lookup"><span data-stu-id="3f778-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="3f778-155">Следующая ошибка свидетельствует о том, что размер передаваемых файлов превышает настроенное на сервере значение `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="3f778-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="3f778-156">Значение по умолчанию составляет `30000000`, что приблизительно равно 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="3f778-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="3f778-157">Это значение можно настроить, внеся изменение в файл *web.config*:</span><span class="sxs-lookup"><span data-stu-id="3f778-157">The value can be customized by editing *web.config*:</span></span>

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

<span data-ttu-id="3f778-158">Этот параметр применим только к службам IIS.</span><span class="sxs-lookup"><span data-stu-id="3f778-158">This setting only applies to IIS.</span></span> <span data-ttu-id="3f778-159">При размещении в Kestrel такой проблемы возникать не должно.</span><span class="sxs-lookup"><span data-stu-id="3f778-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="3f778-160">Дополнительные сведения см. в статье [Ограничения запроса \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="3f778-160">For more information, see [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="3f778-161">Исключение, связанное с пустой ссылкой в IFormFile</span><span class="sxs-lookup"><span data-stu-id="3f778-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="3f778-162">Если контроллер принимает передаваемые файлы с помощью `IFormFile`, но значение всегда равно NULL, проверьте, указан ли в HTML-форме атрибут `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3f778-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="3f778-163">Если этот атрибут не задан для элемента `<form>`, передача файлов происходить не будет и все связанные аргументы `IFormFile` будут иметь значение NULL.</span><span class="sxs-lookup"><span data-stu-id="3f778-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
