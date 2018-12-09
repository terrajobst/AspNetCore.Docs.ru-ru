---
title: Отправка файлов на страницу Razor в ASP.NET Core
author: guardrex
description: Сведения об отправке файлов на страницу Razor в ASP.NET Core с помощью класса FileUpload.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
ms.custom: seodec18
uid: razor-pages/upload-files
ms.openlocfilehash: 80929c6c1a95b46b942958def1540ac8ed5abc81
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121405"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a>Отправка файлов на страницу Razor в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Этот раздел построен на основе [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) в <xref:tutorials/razor-pages/razor-pages-start>.

В этом разделе показано, как использовать простой привязки модели для передачи файлов, который хорошо подходит для небольших файлов. Сведения о потоковой передаче больших файлов см. в статье [Отправка больших файлов с помощью потоковой передачи](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

В следующих действиях в пример приложения добавляется функция отправки файлов с расписанием фильмов. Расписание фильмов представлено классом `Schedule`. Он включает в себя две версии расписания. Одна версия, `PublicSchedule`, предоставляется клиентам. Для сотрудников организации используется другая версия — `PrivateSchedule`. Каждая версия отправляется в виде отдельного файла. В руководстве показано, как выполнить две отправки файла со страницы на сервер с помощью отдельной операции POST.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>Замечания по безопасности

Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер. Злоумышленник может выполнить атаку типа ["отказ в обслуживании"](/windows-hardware/drivers/ifs/denial-of-service) и другие атаки на систему. Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак:

* Отправьте файлы в область отправки выделенных файлов в системе. Это упрощает распространение мер безопасности на передаваемое содержимое. Разрешая передачу файлов, убедитесь, что разрешения на выполнение отключены в расположении отправки.
* Используйте безопасное имя файла, определенное приложением, а не введенное пользователем имя или имя отправленного файла.
* Разрешите только определенный набор одобренных расширений файлов.
* Убедитесь, что на сервере выполняются проверки на стороне клиента. Проверки на стороне клиента можно легко обойти.
* Проверьте размер отправляемых файлов и запретите выполнять отправку файлов большего размера.
* Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ.

> [!WARNING]
> Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:
> * полностью захватить управление системой;
> * перезагрузить систему так, что система окажется в неработоспособном состоянии;
> * скомпрометировать пользовательские или системные данные;
> * применить граффити к открытому интерфейсу.

## <a name="add-a-fileupload-class"></a>Добавление класса FileUpload

Создайте страницу Razor для обработки парной отправки файлов. Добавьте класс `FileUpload`, привязанный к странице для получения данных расписания. Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Назовите класс **FileUpload** и добавьте следующие свойства.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

Этот класс содержит свойство для заголовка расписания и свойство для каждой из двух версий расписания. Все три свойства являются обязательными, а заголовок должен иметь длину от 3 до 60 символов.

## <a name="add-a-helper-method-to-upload-files"></a>Добавление вспомогательного метода для отправки файлов

Чтобы избежать дублирования кода для обработки отправленных файлов расписания, сначала добавьте статический вспомогательный метод. Создайте папку *Utilities* в приложении и добавьте файл *FileHelpers.cs* с приведенным ниже содержимым. Вспомогательный метод `ProcessFormFile` принимает [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) и возвращает строку, содержащую размер и содержимое файла. Выполняется проверка типа содержимого и длины. Если файл не проходит проверку, в `ModelState` добавляется ошибка.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a>Сохранение файла на диск

В примере приложения загруженные файлы сохраняются в полях базы данных. Чтобы сохранить файл на диск, используйте [FileStream](/dotnet/api/system.io.filestream). В следующем примере копируется файл из `FileUpload.UploadPublicSchedule` в `FileStream` в методе `OnPostAsync`. `FileStream` записывает файл на диск в указанном `<PATH-AND-FILE-NAME>`:

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

Рабочий процесс должен иметь разрешения на запись в расположении, определенном с помощью `filePath`.

> [!NOTE]
> `filePath` *должен* содержать имя файла. Если имя файла не указано, в среде выполнения возникает исключение [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception).

> [!WARNING]
> Никогда не сохраняйте переданные файлы в дереве каталогов, где находится приложение.
>
> В образце кода не обеспечена защита на стороне сервера от передачи вредоносных файлов. Сведения об уменьшении контактной зоны атаки во время приема файлов от пользователей см. в следующих ресурсах:
>
> * [Неограниченная отправка файлов](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Безопасность Azure: убедитесь в наличии надлежащих мер контроля при получении файлов от пользователей](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a>Сохранение файла в хранилище BLOB-объектов Azure

Чтобы отправить содержимое файла в хранилище BLOB-объектов Azure, см. руководство по [началу работы с хранилище BLOB-объектов Azure с помощью .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). В статье демонстрируется использование [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) для сохранения [FileStream](/dotnet/api/system.io.filestream) в хранилище больших двоичных объектов.

## <a name="add-the-schedule-class"></a>Добавление класса Schedule

Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Назовите класс **Schedule** и добавьте следующие свойства.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

Класс использует атрибуты `Display` и `DisplayFormat`, которые создают понятные заголовки и форматирование при отрисовке данных расписания.

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a>Обновление RazorPagesMovieContext

Укажите `DbSet` в `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) для расписаний.

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a>Обновление MovieContext

Укажите `DbSet` в `MovieContext` (*Models/MovieContext.cs*) для расписаний:

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a>Добавление таблицы Schedule в базу данных

Откройте консоль диспетчера пакетов (PMC): **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.

![Меню PMC](upload-files/_static/pmc.png)

В PMC выполните указанные ниже команды. Эти команды добавляют таблицу `Schedule` в базу данных.

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Добавление страницы Razor для отправки файлов

В папке *Pages* создайте папку *Schedules*. В папке *Schedules* создайте страницу *Index.cshtml* для отправки расписания со следующим содержимым.

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

Каждая группа формы включает метку **\<label>**, отображающую имя каждого свойства класса. Атрибуты `Display` в модели `FileUpload` предоставляют отображаемые значения для меток. Например, отображаемое имя свойства `UploadPublicSchedule` задается с помощью `[Display(Name="Public Schedule")]`, в результате чего при отрисовке формы в метке отображается текст "Public Schedule" (Общее расписание).

Каждая группа формы включает период проверки **\<span>**. Если введенные пользователем данные не соответствуют атрибутам свойства, заданным в классе `FileUpload`, или какой-либо из этапов проверки файла в методе `ProcessFormFile` завершается с ошибкой, модель не проходит проверку. При сбое проверки модели для пользователя выводится сообщение с полезными данными. Например, для свойства `Title` указаны `[Required]` и `[StringLength(60, MinimumLength = 3)]`. Если пользователь не задает заголовок, он получает сообщение, указывающее на обязательный характер этого значения. Если пользователь вводит значение длиной менее 3 или более 60 символов, он получает сообщение о неправильной длине. Если указан файл без содержимого, появляется сообщение о том, что этот файл пуст.

## <a name="add-the-page-model"></a>Добавление страничной модели

Добавьте страничную модель (*Index.cshtml.cs*) в папку *Schedules*.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

Модель страницы (`IndexModel` в *Index.cshtml.cs*) привязывается к классу `FileUpload`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

Модель также использует список расписаний (`IList<Schedule>`) для отображения расписаний, хранящихся в базе данных на странице.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

Когда страница загружается с `OnGetAsync`, `Schedules` заполняется значениями из базы данных и используется для создания HTML-таблицы загруженных расписаний.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

При публикации формы на сервере проверяется `ModelState`. В случае ошибки `Schedule` перестраивается, а страница отрисовывается с отображением одного сообщения или нескольких о том, почему не удалось выполнить проверку страницы. При прохождении проверки свойства `FileUpload` используются в *OnPostAsync*, чтобы передать файлы для двух версий расписания и создать объект `Schedule` для хранения данных. После этого расписание сохраняется в базе данных.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a>Ссылка на страницу Razor для отправки файлов

Откройте *Pages/Shared/_Layout.cshtml* и добавьте ссылку на панель навигации, позволяющую перейти на страницу расписаний.

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Добавление страницы для подтверждения удаления расписания

Когда пользователь запускает операцию удаления расписания, предоставляется возможность ее отмены. Добавьте страницу подтверждения удаления (*Delete.cshtml*) в папку *Schedules*.

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

Страничная модель (*Delete.cshtml.cs*) загружает отдельное расписание, определяемое идентификатором `id` в данных маршрута запроса. Добавьте файл *Delete.cshtml.cs* в папку *Schedules*.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

Метод `OnPostAsync` обрабатывает удаление расписания по его `id`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

После успешного удаления расписания `RedirectToPage` направляет пользователя обратно на страницу расписаний *Index.cshtml*.

## <a name="the-working-schedules-razor-page"></a>Рабочая страница Razor для расписаний

При загрузке страницы метки и входные данные для заголовка расписания, общедоступного расписания и частного расписания отрисовываются с помощью кнопки отправки.

![Страница Razor в том виде, в котором она отображается при начальной загрузке, без ошибок проверки и пустых полей](upload-files/_static/browser1.png)

Нажатие кнопки **Upload** (Отправить) без заполнения полей нарушает атрибуты `[Required]` в модели. `ModelState` не является допустимым. Для пользователя отображаются сообщения об ошибках проверки

![Сообщения об ошибках проверки отображаются рядом с каждым элементом управления ввода.](upload-files/_static/browser2.png)

Введите две буквы в поле **Title** (Заголовок). Сообщение о проверке изменяется, указывая, что заголовок должен иметь длину от 3 до 60 символов.

![Изменение сообщения о проверке заголовка](upload-files/_static/browser3.png)

При отправке одного расписания или нескольких они отображаются в разделе **Loaded Schedules** (Загруженные расписания).

![Таблица загруженных расписаний, показывающая заголовок каждого расписания, дату отправки в формате UTC, размер файла общедоступной версии и размер файла закрытой версии](upload-files/_static/browser4.png)

Пользователь может щелкнуть ссылку **Delete** (Удалить), чтобы перейти в представление подтверждения удаления, где сможет подтвердить или отменить операцию удаления.

## <a name="troubleshooting"></a>Устранение неполадок

Дополнительные сведения с `IFormFile` передачи данных, см. в разделе [передача файлов в ASP.NET Core: Устранение неполадок](xref:mvc/models/file-uploads#troubleshooting).
