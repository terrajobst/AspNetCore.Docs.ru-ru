---
title: Отправка файлов на страницу Razor в ASP.NET Core
author: guardrex
description: Сведения об отправке файлов на страницу Razor
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 92e72869967b6e3202c97b92e341ea22adc69651
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912505"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="0f1bc-103">Отправка файлов на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f1bc-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="0f1bc-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="0f1bc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f1bc-105">Этот раздел построен на основе [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) в <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="0f1bc-106">В этом разделе показано, как использовать простой привязки модели для передачи файлов, который хорошо подходит для небольших файлов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="0f1bc-107">Сведения о потоковой передаче больших файлов см. в статье [Отправка больших файлов с помощью потоковой передачи](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="0f1bc-108">В следующих действиях в пример приложения добавляется функция отправки файлов с расписанием фильмов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="0f1bc-109">Расписание фильмов представлено классом `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="0f1bc-110">Он включает в себя две версии расписания.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="0f1bc-111">Одна версия, `PublicSchedule`, предоставляется клиентам.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="0f1bc-112">Для сотрудников организации используется другая версия — `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="0f1bc-113">Каждая версия отправляется в виде отдельного файла.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="0f1bc-114">В руководстве показано, как выполнить две отправки файла со страницы на сервер с помощью отдельной операции POST.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0f1bc-115">Замечания по безопасности</span><span class="sxs-lookup"><span data-stu-id="0f1bc-115">Security considerations</span></span>

<span data-ttu-id="0f1bc-116">Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0f1bc-117">Злоумышленник может выполнить атаку типа ["отказ в обслуживании"](/windows-hardware/drivers/ifs/denial-of-service) и другие атаки на систему.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="0f1bc-118">Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак:</span><span class="sxs-lookup"><span data-stu-id="0f1bc-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0f1bc-119">Отправьте файлы в область отправки выделенных файлов в системе. Это упрощает распространение мер безопасности на передаваемое содержимое.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="0f1bc-120">Разрешая передачу файлов, убедитесь, что разрешения на выполнение отключены в расположении отправки.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="0f1bc-121">Используйте безопасное имя файла, определенное приложением, а не введенное пользователем имя или имя отправленного файла.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="0f1bc-122">Разрешите только определенный набор одобренных расширений файлов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="0f1bc-123">Убедитесь, что на сервере выполняются проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="0f1bc-124">Проверки на стороне клиента можно легко обойти.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0f1bc-125">Проверьте размер отправляемых файлов и запретите выполнять отправку файлов большего размера.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="0f1bc-126">Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="0f1bc-127">Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:</span><span class="sxs-lookup"><span data-stu-id="0f1bc-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="0f1bc-128">полностью захватить управление системой;</span><span class="sxs-lookup"><span data-stu-id="0f1bc-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="0f1bc-129">перезагрузить систему так, что система окажется в неработоспособном состоянии;</span><span class="sxs-lookup"><span data-stu-id="0f1bc-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="0f1bc-130">скомпрометировать пользовательские или системные данные;</span><span class="sxs-lookup"><span data-stu-id="0f1bc-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="0f1bc-131">применить граффити к открытому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="0f1bc-132">Добавление класса FileUpload</span><span class="sxs-lookup"><span data-stu-id="0f1bc-132">Add a FileUpload class</span></span>

<span data-ttu-id="0f1bc-133">Создайте страницу Razor для обработки парной отправки файлов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="0f1bc-134">Добавьте класс `FileUpload`, привязанный к странице для получения данных расписания.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="0f1bc-135">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-135">Right click the *Models* folder.</span></span> <span data-ttu-id="0f1bc-136">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="0f1bc-137">Назовите класс **FileUpload** и добавьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-137">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="0f1bc-138">Этот класс содержит свойство для заголовка расписания и свойство для каждой из двух версий расписания.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="0f1bc-139">Все три свойства являются обязательными, а заголовок должен иметь длину от 3 до 60 символов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="0f1bc-140">Добавление вспомогательного метода для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="0f1bc-140">Add a helper method to upload files</span></span>

<span data-ttu-id="0f1bc-141">Чтобы избежать дублирования кода для обработки отправленных файлов расписания, сначала добавьте статический вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="0f1bc-142">Создайте папку *Utilities* в приложении и добавьте файл *FileHelpers.cs* с приведенным ниже содержимым.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="0f1bc-143">Вспомогательный метод `ProcessFormFile` принимает [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) и возвращает строку, содержащую размер и содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="0f1bc-144">Выполняется проверка типа содержимого и длины.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-144">The content type and length are checked.</span></span> <span data-ttu-id="0f1bc-145">Если файл не проходит проверку, в `ModelState` добавляется ошибка.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="0f1bc-146">Сохранение файла на диск</span><span class="sxs-lookup"><span data-stu-id="0f1bc-146">Save the file to disk</span></span>

<span data-ttu-id="0f1bc-147">В примере приложения загруженные файлы сохраняются в полях базы данных.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="0f1bc-148">Чтобы сохранить файл на диск, используйте [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="0f1bc-149">В следующем примере копируется файл из `FileUpload.UploadPublicSchedule` в `FileStream` в методе `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="0f1bc-150">`FileStream` записывает файл на диск в указанном `<PATH-AND-FILE-NAME>`:</span><span class="sxs-lookup"><span data-stu-id="0f1bc-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

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

<span data-ttu-id="0f1bc-151">Рабочий процесс должен иметь разрешения на запись в расположении, определенном с помощью `filePath`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="0f1bc-152">`filePath` *должен* содержать имя файла.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="0f1bc-153">Если имя файла не указано, в среде выполнения возникает исключение [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="0f1bc-154">Никогда не сохраняйте переданные файлы в дереве каталогов, где находится приложение.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="0f1bc-155">В образце кода не обеспечена защита на стороне сервера от передачи вредоносных файлов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="0f1bc-156">Сведения об уменьшении контактной зоны атаки во время приема файлов от пользователей см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="0f1bc-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="0f1bc-157">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="0f1bc-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="0f1bc-158">Безопасность Azure: убедитесь в наличии надлежащих мер контроля при получении файлов от пользователей</span><span class="sxs-lookup"><span data-stu-id="0f1bc-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="0f1bc-159">Сохранение файла в хранилище BLOB-объектов Azure</span><span class="sxs-lookup"><span data-stu-id="0f1bc-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="0f1bc-160">Чтобы отправить содержимое файла в хранилище BLOB-объектов Azure, см. руководство по [началу работы с хранилище BLOB-объектов Azure с помощью .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="0f1bc-161">В статье демонстрируется использование [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) для сохранения [FileStream](/dotnet/api/system.io.filestream) в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="0f1bc-162">Добавление класса Schedule</span><span class="sxs-lookup"><span data-stu-id="0f1bc-162">Add the Schedule class</span></span>

<span data-ttu-id="0f1bc-163">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-163">Right click the *Models* folder.</span></span> <span data-ttu-id="0f1bc-164">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="0f1bc-165">Назовите класс **Schedule** и добавьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-165">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="0f1bc-166">Класс использует атрибуты `Display` и `DisplayFormat`, которые создают понятные заголовки и форматирование при отрисовке данных расписания.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="0f1bc-167">Обновление RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="0f1bc-167">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="0f1bc-168">Укажите `DbSet` в `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) для расписаний.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-168">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="0f1bc-169">Обновление MovieContext</span><span class="sxs-lookup"><span data-stu-id="0f1bc-169">Update the MovieContext</span></span>

<span data-ttu-id="0f1bc-170">Укажите `DbSet` в `MovieContext` (*Models/MovieContext.cs*) для расписаний:</span><span class="sxs-lookup"><span data-stu-id="0f1bc-170">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="0f1bc-171">Добавление таблицы Schedule в базу данных</span><span class="sxs-lookup"><span data-stu-id="0f1bc-171">Add the Schedule table to the database</span></span>

<span data-ttu-id="0f1bc-172">Откройте консоль диспетчера пакетов (PMC): **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-172">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Меню PMC](upload-files/_static/pmc.png)

<span data-ttu-id="0f1bc-174">В PMC выполните указанные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-174">In the PMC, execute the following commands.</span></span> <span data-ttu-id="0f1bc-175">Эти команды добавляют таблицу `Schedule` в базу данных.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-175">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="0f1bc-176">Добавление страницы Razor для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="0f1bc-176">Add a file upload Razor Page</span></span>

<span data-ttu-id="0f1bc-177">В папке *Pages* создайте папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-177">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="0f1bc-178">В папке *Schedules* создайте страницу *Index.cshtml* для отправки расписания со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-178">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="0f1bc-179">Каждая группа формы включает метку **\<label>**, отображающую имя каждого свойства класса.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-179">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="0f1bc-180">Атрибуты `Display` в модели `FileUpload` предоставляют отображаемые значения для меток.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-180">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="0f1bc-181">Например, отображаемое имя свойства `UploadPublicSchedule` задается с помощью `[Display(Name="Public Schedule")]`, в результате чего при отрисовке формы в метке отображается текст "Public Schedule" (Общее расписание).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-181">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="0f1bc-182">Каждая группа формы включает период проверки **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-182">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="0f1bc-183">Если введенные пользователем данные не соответствуют атрибутам свойства, заданным в классе `FileUpload`, или какой-либо из этапов проверки файла в методе `ProcessFormFile` завершается с ошибкой, модель не проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-183">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="0f1bc-184">При сбое проверки модели для пользователя выводится сообщение с полезными данными.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-184">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="0f1bc-185">Например, для свойства `Title` указаны `[Required]` и `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-185">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="0f1bc-186">Если пользователь не задает заголовок, он получает сообщение, указывающее на обязательный характер этого значения.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-186">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="0f1bc-187">Если пользователь вводит значение длиной менее 3 или более 60 символов, он получает сообщение о неправильной длине.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-187">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="0f1bc-188">Если указан файл без содержимого, появляется сообщение о том, что этот файл пуст.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-188">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="0f1bc-189">Добавление страничной модели</span><span class="sxs-lookup"><span data-stu-id="0f1bc-189">Add the page model</span></span>

<span data-ttu-id="0f1bc-190">Добавьте страничную модель (*Index.cshtml.cs*) в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-190">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="0f1bc-191">Модель страницы (`IndexModel` в *Index.cshtml.cs*) привязывается к классу `FileUpload`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-191">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0f1bc-192">Модель также использует список расписаний (`IList<Schedule>`) для отображения расписаний, хранящихся в базе данных на странице.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-192">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="0f1bc-193">Когда страница загружается с `OnGetAsync`, `Schedules` заполняется значениями из базы данных и используется для создания HTML-таблицы загруженных расписаний.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-193">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="0f1bc-194">При публикации формы на сервере проверяется `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-194">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="0f1bc-195">В случае ошибки `Schedule` перестраивается, а страница отрисовывается с отображением одного сообщения или нескольких о том, почему не удалось выполнить проверку страницы.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-195">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="0f1bc-196">При прохождении проверки свойства `FileUpload` используются в *OnPostAsync*, чтобы передать файлы для двух версий расписания и создать объект `Schedule` для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-196">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="0f1bc-197">После этого расписание сохраняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-197">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="0f1bc-198">Ссылка на страницу Razor для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="0f1bc-198">Link the file upload Razor Page</span></span>

<span data-ttu-id="0f1bc-199">Откройте *Pages/Shared/_Layout.cshtml* и добавьте ссылку на панель навигации, позволяющую перейти на страницу расписаний.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-199">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

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

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="0f1bc-200">Добавление страницы для подтверждения удаления расписания</span><span class="sxs-lookup"><span data-stu-id="0f1bc-200">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="0f1bc-201">Когда пользователь запускает операцию удаления расписания, предоставляется возможность ее отмены.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-201">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="0f1bc-202">Добавьте страницу подтверждения удаления (*Delete.cshtml*) в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-202">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="0f1bc-203">Страничная модель (*Delete.cshtml.cs*) загружает отдельное расписание, определяемое идентификатором `id` в данных маршрута запроса.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-203">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="0f1bc-204">Добавьте файл *Delete.cshtml.cs* в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-204">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="0f1bc-205">Метод `OnPostAsync` обрабатывает удаление расписания по его `id`.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-205">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="0f1bc-206">После успешного удаления расписания `RedirectToPage` направляет пользователя обратно на страницу расписаний *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-206">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="0f1bc-207">Рабочая страница Razor для расписаний</span><span class="sxs-lookup"><span data-stu-id="0f1bc-207">The working Schedules Razor Page</span></span>

<span data-ttu-id="0f1bc-208">При загрузке страницы метки и входные данные для заголовка расписания, общедоступного расписания и частного расписания отрисовываются с помощью кнопки отправки.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-208">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Страница Razor в том виде, в котором она отображается при начальной загрузке, без ошибок проверки и пустых полей](upload-files/_static/browser1.png)

<span data-ttu-id="0f1bc-210">Нажатие кнопки **Upload** (Отправить) без заполнения полей нарушает атрибуты `[Required]` в модели.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-210">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="0f1bc-211">`ModelState` не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-211">The `ModelState` is invalid.</span></span> <span data-ttu-id="0f1bc-212">Для пользователя отображаются сообщения об ошибках проверки</span><span class="sxs-lookup"><span data-stu-id="0f1bc-212">The validation error messages are displayed to the user:</span></span>

![Сообщения об ошибках проверки отображаются рядом с каждым элементом управления ввода.](upload-files/_static/browser2.png)

<span data-ttu-id="0f1bc-214">Введите две буквы в поле **Title** (Заголовок).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-214">Type two letters into the **Title** field.</span></span> <span data-ttu-id="0f1bc-215">Сообщение о проверке изменяется, указывая, что заголовок должен иметь длину от 3 до 60 символов.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-215">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Изменение сообщения о проверке заголовка](upload-files/_static/browser3.png)

<span data-ttu-id="0f1bc-217">При отправке одного расписания или нескольких они отображаются в разделе **Loaded Schedules** (Загруженные расписания).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-217">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Таблица загруженных расписаний, показывающая заголовок каждого расписания, дату отправки в формате UTC, размер файла общедоступной версии и размер файла закрытой версии](upload-files/_static/browser4.png)

<span data-ttu-id="0f1bc-219">Пользователь может щелкнуть ссылку **Delete** (Удалить), чтобы перейти в представление подтверждения удаления, где сможет подтвердить или отменить операцию удаления.</span><span class="sxs-lookup"><span data-stu-id="0f1bc-219">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0f1bc-220">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="0f1bc-220">Troubleshooting</span></span>

<span data-ttu-id="0f1bc-221">Дополнительные сведения с `IFormFile` передачи данных, см. в разделе [передача файлов в ASP.NET Core: Устранение неполадок](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="0f1bc-221">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
