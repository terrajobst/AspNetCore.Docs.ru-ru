---
title: "Отправка файлов на страницу Razor в ASP.NET Core"
author: guardrex
description: "Сведения об отправке файлов на страницу Razor"
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="962c3-103">Отправка файлов на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="962c3-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="962c3-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="962c3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="962c3-105">В этом разделе описана отправка файлов с использованием страницы Razor.</span><span class="sxs-lookup"><span data-stu-id="962c3-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="962c3-106">[Пример приложения Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) в этом руководстве использует для отправки файлов простую привязку модели, которая хорошо подходит для небольших файлов.</span><span class="sxs-lookup"><span data-stu-id="962c3-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="962c3-107">Сведения о потоковой передаче больших файлов см. в статье [Отправка больших файлов с помощью потоковой передачи](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="962c3-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="962c3-108">В следующих действиях в пример приложения добавляется функция отправки файлов с расписанием фильмов.</span><span class="sxs-lookup"><span data-stu-id="962c3-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="962c3-109">Расписание фильмов представлено классом `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="962c3-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="962c3-110">Он включает в себя две версии расписания.</span><span class="sxs-lookup"><span data-stu-id="962c3-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="962c3-111">Одна версия, `PublicSchedule`, предоставляется клиентам.</span><span class="sxs-lookup"><span data-stu-id="962c3-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="962c3-112">Для сотрудников организации используется другая версия — `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="962c3-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="962c3-113">Каждая версия отправляется в виде отдельного файла.</span><span class="sxs-lookup"><span data-stu-id="962c3-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="962c3-114">В руководстве показано, как выполнить две отправки файла со страницы на сервер с помощью отдельной операции POST.</span><span class="sxs-lookup"><span data-stu-id="962c3-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="962c3-115">Замечания по безопасности</span><span class="sxs-lookup"><span data-stu-id="962c3-115">Security considerations</span></span>

<span data-ttu-id="962c3-116">Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер.</span><span class="sxs-lookup"><span data-stu-id="962c3-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="962c3-117">Злоумышленник может выполнить атаку типа ["отказ в обслуживании"](/windows-hardware/drivers/ifs/denial-of-service) и другие атаки на систему.</span><span class="sxs-lookup"><span data-stu-id="962c3-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="962c3-118">Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак:</span><span class="sxs-lookup"><span data-stu-id="962c3-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="962c3-119">Отправьте файлы в область отправки выделенных файлов в системе. Это упрощает распространение мер безопасности на передаваемое содержимое.</span><span class="sxs-lookup"><span data-stu-id="962c3-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="962c3-120">Разрешая передачу файлов, убедитесь, что разрешения на выполнение отключены в расположении отправки.</span><span class="sxs-lookup"><span data-stu-id="962c3-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="962c3-121">Используйте безопасное имя файла, определенное приложением, а не введенное пользователем имя или имя отправленного файла.</span><span class="sxs-lookup"><span data-stu-id="962c3-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="962c3-122">Разрешите только определенный набор одобренных расширений файлов.</span><span class="sxs-lookup"><span data-stu-id="962c3-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="962c3-123">Убедитесь, что на сервере выполняются проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="962c3-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="962c3-124">Проверки на стороне клиента можно легко обойти.</span><span class="sxs-lookup"><span data-stu-id="962c3-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="962c3-125">Проверьте размер отправляемых файлов и запретите выполнять отправку файлов большего размера.</span><span class="sxs-lookup"><span data-stu-id="962c3-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="962c3-126">Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ.</span><span class="sxs-lookup"><span data-stu-id="962c3-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="962c3-127">Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:</span><span class="sxs-lookup"><span data-stu-id="962c3-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="962c3-128">полностью захватить управление системой;</span><span class="sxs-lookup"><span data-stu-id="962c3-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="962c3-129">перезагрузить систему так, что система окажется в неработоспособном состоянии;</span><span class="sxs-lookup"><span data-stu-id="962c3-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="962c3-130">скомпрометировать пользовательские или системные данные;</span><span class="sxs-lookup"><span data-stu-id="962c3-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="962c3-131">применить граффити к открытому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="962c3-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="962c3-132">Добавление класса FileUpload</span><span class="sxs-lookup"><span data-stu-id="962c3-132">Add a FileUpload class</span></span>

<span data-ttu-id="962c3-133">Создайте страницу Razor для обработки парной отправки файлов.</span><span class="sxs-lookup"><span data-stu-id="962c3-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="962c3-134">Добавьте класс `FileUpload`, привязанный к странице для получения данных расписания.</span><span class="sxs-lookup"><span data-stu-id="962c3-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="962c3-135">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="962c3-135">Right click the *Models* folder.</span></span> <span data-ttu-id="962c3-136">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="962c3-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="962c3-137">Назовите класс **FileUpload** и добавьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="962c3-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="962c3-138">Этот класс содержит свойство для заголовка расписания и свойство для каждой из двух версий расписания.</span><span class="sxs-lookup"><span data-stu-id="962c3-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="962c3-139">Все три свойства являются обязательными, а заголовок должен иметь длину от 3 до 60 символов.</span><span class="sxs-lookup"><span data-stu-id="962c3-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="962c3-140">Добавление вспомогательного метода для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="962c3-140">Add a helper method to upload files</span></span>

<span data-ttu-id="962c3-141">Чтобы избежать дублирования кода для обработки отправленных файлов расписания, сначала добавьте статический вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="962c3-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="962c3-142">Создайте папку *Utilities* в приложении и добавьте файл *FileHelpers.cs* с приведенным ниже содержимым.</span><span class="sxs-lookup"><span data-stu-id="962c3-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="962c3-143">Вспомогательный метод `ProcessFormFile` принимает [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) и возвращает строку, содержащую размер и содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="962c3-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="962c3-144">Выполняется проверка типа содержимого и длины.</span><span class="sxs-lookup"><span data-stu-id="962c3-144">The content type and length are checked.</span></span> <span data-ttu-id="962c3-145">Если файл не проходит проверку, в `ModelState` добавляется ошибка.</span><span class="sxs-lookup"><span data-stu-id="962c3-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="962c3-146">Сохранение файла на диск</span><span class="sxs-lookup"><span data-stu-id="962c3-146">Save the file to disk</span></span>

<span data-ttu-id="962c3-147">Это пример приложения сохраняет содержимое файла в поле базы данных.</span><span class="sxs-lookup"><span data-stu-id="962c3-147">The sample app saves the file's content into a database field.</span></span> <span data-ttu-id="962c3-148">Чтобы сохранить содержимое файла на диск, используйте [FileStream](/dotnet/api/system.io.filestream):</span><span class="sxs-lookup"><span data-stu-id="962c3-148">To save the file's content to disk, use a [FileStream](/dotnet/api/system.io.filestream):</span></span>

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

<span data-ttu-id="962c3-149">Рабочий процесс должен иметь разрешения на запись в расположении, определенном с помощью `filePath`.</span><span class="sxs-lookup"><span data-stu-id="962c3-149">The worker process must have write permissions to the location specified by `filePath`.</span></span>

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="962c3-150">Сохранение файла в хранилище BLOB-объектов Azure</span><span class="sxs-lookup"><span data-stu-id="962c3-150">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="962c3-151">Чтобы отправить содержимое файла в хранилище BLOB-объектов Azure, см. руководство по [началу работы с хранилище BLOB-объектов Azure с помощью .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="962c3-151">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="962c3-152">В статье демонстрируется использование [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) для сохранения [FileStream](/dotnet/api/system.io.filestream) в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="962c3-152">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="962c3-153">Добавление класса Schedule</span><span class="sxs-lookup"><span data-stu-id="962c3-153">Add the Schedule class</span></span>

<span data-ttu-id="962c3-154">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="962c3-154">Right click the *Models* folder.</span></span> <span data-ttu-id="962c3-155">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="962c3-155">Select **Add** > **Class**.</span></span> <span data-ttu-id="962c3-156">Назовите класс **Schedule** и добавьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="962c3-156">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="962c3-157">Класс использует атрибуты `Display` и `DisplayFormat`, которые создают понятные заголовки и форматирование при отрисовке данных расписания.</span><span class="sxs-lookup"><span data-stu-id="962c3-157">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="962c3-158">Обновление MovieContext</span><span class="sxs-lookup"><span data-stu-id="962c3-158">Update the MovieContext</span></span>

<span data-ttu-id="962c3-159">Укажите `DbSet` в `MovieContext` (*Models/MovieContext.cs*) для расписаний:</span><span class="sxs-lookup"><span data-stu-id="962c3-159">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="962c3-160">Добавление таблицы Schedule в базу данных</span><span class="sxs-lookup"><span data-stu-id="962c3-160">Add the Schedule table to the database</span></span>

<span data-ttu-id="962c3-161">Откройте консоль диспетчера пакетов (PMC): **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="962c3-161">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="962c3-163">В PMC выполните указанные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="962c3-163">In the PMC, execute the following commands.</span></span> <span data-ttu-id="962c3-164">Эти команды добавляют таблицу `Schedule` в базу данных.</span><span class="sxs-lookup"><span data-stu-id="962c3-164">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="962c3-165">Добавление страницы Razor для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="962c3-165">Add a file upload Razor Page</span></span>

<span data-ttu-id="962c3-166">В папке *Pages* создайте папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="962c3-166">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="962c3-167">В папке *Schedules* создайте страницу *Index.cshtml* для отправки расписания со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="962c3-167">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="962c3-168">Каждая группа формы включает метку **\<label>**, отображающую имя каждого свойства класса.</span><span class="sxs-lookup"><span data-stu-id="962c3-168">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="962c3-169">Атрибуты `Display` в модели `FileUpload` предоставляют отображаемые значения для меток.</span><span class="sxs-lookup"><span data-stu-id="962c3-169">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="962c3-170">Например, отображаемое имя свойства `UploadPublicSchedule` задается с помощью `[Display(Name="Public Schedule")]`, в результате чего при отрисовке формы в метке отображается текст "Public Schedule" (Общее расписание).</span><span class="sxs-lookup"><span data-stu-id="962c3-170">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="962c3-171">Каждая группа формы включает период проверки **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="962c3-171">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="962c3-172">Если введенные пользователем данные не соответствуют атрибутам свойства, заданным в классе `FileUpload`, или какой-либо из этапов проверки файла в методе `ProcessFormFile` завершается с ошибкой, модель не проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="962c3-172">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="962c3-173">При сбое проверки модели для пользователя выводится сообщение с полезными данными.</span><span class="sxs-lookup"><span data-stu-id="962c3-173">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="962c3-174">Например, для свойства `Title` указаны `[Required]` и `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="962c3-174">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="962c3-175">Если пользователь не задает заголовок, он получает сообщение, указывающее на обязательный характер этого значения.</span><span class="sxs-lookup"><span data-stu-id="962c3-175">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="962c3-176">Если пользователь вводит значение длиной менее 3 или более 60 символов, он получает сообщение о неправильной длине.</span><span class="sxs-lookup"><span data-stu-id="962c3-176">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="962c3-177">Если указан файл без содержимого, появляется сообщение о том, что этот файл пуст.</span><span class="sxs-lookup"><span data-stu-id="962c3-177">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="962c3-178">Добавление страничной модели</span><span class="sxs-lookup"><span data-stu-id="962c3-178">Add the page model</span></span>

<span data-ttu-id="962c3-179">Добавьте страничную модель (*Index.cshtml.cs*) в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="962c3-179">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="962c3-180">Модель страницы (`IndexModel` в *Index.cshtml.cs*) привязывается к классу `FileUpload`.</span><span class="sxs-lookup"><span data-stu-id="962c3-180">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="962c3-181">Модель также использует список расписаний (`IList<Schedule>`) для отображения расписаний, хранящихся в базе данных на странице.</span><span class="sxs-lookup"><span data-stu-id="962c3-181">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="962c3-182">Когда страница загружается с `OnGetAsync`, `Schedules` заполняется значениями из базы данных и используется для создания HTML-таблицы загруженных расписаний.</span><span class="sxs-lookup"><span data-stu-id="962c3-182">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="962c3-183">При публикации формы на сервере проверяется `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="962c3-183">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="962c3-184">В случае ошибки `Schedule` перестраивается, а страница отрисовывается с отображением одного сообщения или нескольких о том, почему не удалось выполнить проверку страницы.</span><span class="sxs-lookup"><span data-stu-id="962c3-184">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="962c3-185">При прохождении проверки свойства `FileUpload` используются в *OnPostAsync*, чтобы передать файлы для двух версий расписания и создать объект `Schedule` для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="962c3-185">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="962c3-186">После этого расписание сохраняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="962c3-186">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="962c3-187">Ссылка на страницу Razor для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="962c3-187">Link the file upload Razor Page</span></span>

<span data-ttu-id="962c3-188">Откройте *_Layout.cshtml* и добавьте ссылку на панель навигации, позволяющую перейти на страницу отправки файлов.</span><span class="sxs-lookup"><span data-stu-id="962c3-188">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="962c3-189">Добавление страницы для подтверждения удаления расписания</span><span class="sxs-lookup"><span data-stu-id="962c3-189">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="962c3-190">Когда пользователь запускает операцию удаления расписания, предоставляется возможность ее отмены.</span><span class="sxs-lookup"><span data-stu-id="962c3-190">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="962c3-191">Добавьте страницу подтверждения удаления (*Delete.cshtml*) в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="962c3-191">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="962c3-192">Страничная модель (*Delete.cshtml.cs*) загружает отдельное расписание, определяемое идентификатором `id` в данных маршрута запроса.</span><span class="sxs-lookup"><span data-stu-id="962c3-192">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="962c3-193">Добавьте файл *Delete.cshtml.cs* в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="962c3-193">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="962c3-194">Метод `OnPostAsync` обрабатывает удаление расписания по его `id`.</span><span class="sxs-lookup"><span data-stu-id="962c3-194">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="962c3-195">После успешного удаления расписания `RedirectToPage` направляет пользователя обратно на страницу расписаний *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="962c3-195">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="962c3-196">Рабочая страница Razor для расписаний</span><span class="sxs-lookup"><span data-stu-id="962c3-196">The working Schedules Razor Page</span></span>

<span data-ttu-id="962c3-197">При загрузке страницы метки и входные данные для заголовка расписания, общедоступного расписания и частного расписания отрисовываются с помощью кнопки отправки.</span><span class="sxs-lookup"><span data-stu-id="962c3-197">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Страница Razor в том виде, в котором она отображается при начальной загрузке, без ошибок проверки и пустых полей](uploading-files/_static/browser1.png)

<span data-ttu-id="962c3-199">Нажатие кнопки **Upload** (Отправить) без заполнения полей нарушает атрибуты `[Required]` в модели.</span><span class="sxs-lookup"><span data-stu-id="962c3-199">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="962c3-200">`ModelState` не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="962c3-200">The `ModelState` is invalid.</span></span> <span data-ttu-id="962c3-201">Для пользователя отображаются сообщения об ошибках проверки</span><span class="sxs-lookup"><span data-stu-id="962c3-201">The validation error messages are displayed to the user:</span></span>

![Сообщения об ошибках проверки отображаются рядом с каждым элементом управления ввода.](uploading-files/_static/browser2.png)

<span data-ttu-id="962c3-203">Введите две буквы в поле **Title** (Заголовок).</span><span class="sxs-lookup"><span data-stu-id="962c3-203">Type two letters into the **Title** field.</span></span> <span data-ttu-id="962c3-204">Сообщение о проверке изменяется, указывая, что заголовок должен иметь длину от 3 до 60 символов.</span><span class="sxs-lookup"><span data-stu-id="962c3-204">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Изменение сообщения о проверке заголовка](uploading-files/_static/browser3.png)

<span data-ttu-id="962c3-206">При отправке одного расписания или нескольких они отображаются в разделе **Loaded Schedules** (Загруженные расписания).</span><span class="sxs-lookup"><span data-stu-id="962c3-206">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Таблица загруженных расписаний, показывающая заголовок каждого расписания, дату отправки в формате UTC, размер файла общедоступной версии и размер файла закрытой версии](uploading-files/_static/browser4.png)

<span data-ttu-id="962c3-208">Пользователь может щелкнуть ссылку **Delete** (Удалить), чтобы перейти в представление подтверждения удаления, где сможет подтвердить или отменить операцию удаления.</span><span class="sxs-lookup"><span data-stu-id="962c3-208">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="962c3-209">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="962c3-209">Troubleshooting</span></span>

<span data-ttu-id="962c3-210">Сведения об устранении неполадок с отправкой `IFormFile` см. в статье [Передача файлов в ASP.NET Core: устранение неполадок](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="962c3-210">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="962c3-211">Благодарим вас за изучение общих сведений о страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="962c3-211">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="962c3-212">Мы благодарны за ваш отзыв!</span><span class="sxs-lookup"><span data-stu-id="962c3-212">We appreciate feedback.</span></span> <span data-ttu-id="962c3-213">Отличным дополнением к этому учебнику является статья [Начало работы с EF и MVC](xref:data/ef-mvc/intro).</span><span class="sxs-lookup"><span data-stu-id="962c3-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="962c3-214">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="962c3-214">Additional resources</span></span>

* [<span data-ttu-id="962c3-215">Передача файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="962c3-215">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="962c3-216">IFormFile</span><span class="sxs-lookup"><span data-stu-id="962c3-216">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="962c3-217">Предыдущая тема. Проверка</span><span class="sxs-lookup"><span data-stu-id="962c3-217">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
