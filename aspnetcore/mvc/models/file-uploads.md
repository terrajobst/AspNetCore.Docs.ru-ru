---
title: Передача файлов в ASP.NET Core
author: ardalis
description: Сведения об использовании привязки модели и потоковой передачи для передачи файлов в ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 7ba4f6d9e3901c310fe9fa7a70382d9243d8b347
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="file-uploads-in-aspnet-core"></a>Передача файлов в ASP.NET Core

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

Действия ASP.NET MVC поддерживают передачу одного или нескольких файлов с помощью простой привязки модели для небольших файлов или потоковой передачи для более крупных файлов.

[Просмотреть или скачать образец с GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Передача небольших файлов с помощью привязки модели

Для передачи небольших файлов можно применить составную HTML-форму или сформировать запрос POST на языке JavaScript. Примеры формы, использующей синтаксис Razor и поддерживающей передачу нескольких файлов, приведен ниже.

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

Для поддержки передачи файлов в HTML-формах должен указываться атрибут `enctype` со значением `multipart/form-data`. Показанный выше элемент ввода `files` поддерживает передачу нескольких файлов. Чтобы разрешить передачу только одного файла, пропустите атрибут `multiple` в этом элементе. Приведенная выше разметка отрисовывается в браузере следующим образом:

![Форма для отправки файла](file-uploads/_static/upload-form.png)

Доступ к отдельным файлам, переданным на сервер, можно получать посредством [привязки модели](xref:mvc/models/model-binding) с помощью интерфейса [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile). `IFormFile` имеет следующую структуру:

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
> Свойство `FileName` требует обязательной проверки. Свойство `FileName` следует использовать только в целях отображения данных.

При передаче файлов с помощью привязки модели и интерфейса `IFormFile` метод действия может принимать либо одиночный интерфейс `IFormFile`, либо интерфейс `IEnumerable<IFormFile>` (или `List<IFormFile>`), представляющий несколько файлов. В приведенном ниже примере осуществляется перебор одного или нескольких переданных файлов, после чего они сохраняются в локальной файловой системе, и возвращается количество и общий размер переданных файлов.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Файлы, передаваемые с помощью интерфейса `IFormFile`, буферизуются в памяти или на диске на веб-сервере перед обработкой. Внутри метода действия содержимое `IFormFile` доступно в виде потока. Помимо локальной файловой системы, файлы могут передаваться в потоковом режиме в [хранилище BLOB-объектов Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) или в [Entity Framework](https://docs.microsoft.com/ef/core/index).

Для сохранения данных двоичных файлов в базе данных с помощью Entity Framework определите для сущности свойство типа `byte[]`:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Определите свойство viewmodel типа `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` можно использовать непосредственно как параметр метода действия или как свойство viewmodel, как показано выше.

Скопируйте экземпляр `IFormFile` в поток и сохраните его в байтовом массиве:

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
> При сохранении двоичных данных в реляционных базах данных следует соблюдать осторожность, так как это может отрицательно сказаться на производительности.

## <a name="uploading-large-files-with-streaming"></a>Передача больших файлов с помощью потоковой передачи

Если из-за размера передаваемых файлов или частоты их передачи в приложении возникают проблемы с ресурсами, рекомендуется применять потоковую передачу вместо буферизации файлов целиком, как в случае с описанным выше подходом на основе привязки модели. Использовать интерфейс `IFormFile` и привязку модели гораздо проще, в то время как для правильной реализации потоковой передачи требуется ряд действий.

> [!NOTE]
> Если размер отдельного буферизуемого файла превышает 64 КБ, он перемещается из оперативной памяти во временный файл на диске на сервере. Потребление ресурсов (диска, ОЗУ) при передаче файлов зависит от количества и размера одновременно передаваемых файлов. Потоковая передача призвана в первую очередь не повысить производительность, а обеспечить масштабирование. При попытке поместить в буфер слишком много файлов может произойти аварийное завершение работы сайта из-за нехватки памяти или места на диске.

В приведенном ниже примере демонстрируется использование JavaScript и Angular для потоковой передачи в действие контроллера. Токен против подделки файла создается с помощью пользовательского атрибута фильтра и передается в заголовках HTTP, а не в теле запроса. Так как метод действия обрабатывает передаваемые данные напрямую, привязка модели отключается другим фильтром. Внутри действия содержимое формы считывается с помощью объекта `MultipartReader`, который считывает каждый объект `MultipartSection` по отдельности, обрабатывая файл или сохраняя содержимое. После считывания всех разделов действие выполняет собственную привязку модели.

Начальное действие загружает форму и сохраняет токен против подделки в файле cookie (с помощью атрибута `GenerateAntiforgeryTokenCookieForAjax`).

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

Атрибут использует встроенную поддержку [защиты от подделки](xref:security/anti-request-forgery) в ASP.NET Core, чтобы задать файл cookie с токеном запроса.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular автоматически передает токен от подделки в заголовке запроса с именем `X-XSRF-TOKEN`. Приложение ASP.NET Core MVC настроено так, что оно ссылается на этот заголовок в конфигурации в файле *Startup.cs*:

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

Показанный ниже атрибут `DisableFormValueModelBinding` предназначен для отключения привязки модели для метода действия `Upload`.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Так как привязка модели отключена, метод действия `Upload` не принимает параметров. Он работает напрямую со свойством `Request` объекта `ControllerBase`. Для считывания каждого раздела служит объект `MultipartReader`. Файл сохраняется с именем GUID, а данные типа "ключ — значение" сохраняются в `KeyValueAccumulator`. После считывания всех разделов содержимое `KeyValueAccumulator` используется для привязки данных формы к типу модели.

Ниже представлен полный код метода `Upload`.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Устранение неполадок

Ниже описываются некоторые распространенные проблемы, которые возникают при передаче файлов, и возможные способы их решения.

### <a name="unexpected-not-found-error-with-iis"></a>Непредвиденная ошибка "Не найдено" в службах IIS

Следующая ошибка свидетельствует о том, что размер передаваемых файлов превышает настроенное на сервере значение `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Значение по умолчанию составляет `30000000`, что приблизительно равно 28,6 МБ. Это значение можно настроить, внеся изменение в файл *web.config*:

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

Этот параметр применим только к службам IIS. При размещении в Kestrel такой проблемы возникать не должно. Дополнительные сведения см. в статье [Ограничения запроса \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Исключение, связанное с пустой ссылкой в IFormFile

Если контроллер принимает передаваемые файлы с помощью `IFormFile`, но значение всегда равно NULL, проверьте, указан ли в HTML-форме атрибут `enctype` со значением `multipart/form-data`. Если этот атрибут не задан для элемента `<form>`, передача файлов происходить не будет и все связанные аргументы `IFormFile` будут иметь значение NULL.
