---
title: Передача файлов в ASP.NET Core
author: guardrex
description: Сведения об использовании привязки модели и потоковой передачи для передачи файлов в ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 04e7533aa190a4875d3f66e8665fec16abec48b3
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2019
ms.locfileid: "73462943"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="efd1d-103">Передача файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efd1d-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="efd1d-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham), [Стив Смит](https://ardalis.com/) (Steve Smith) и [Рутгер Сторм](https://github.com/rutix) (Rutger Storm)</span><span class="sxs-lookup"><span data-stu-id="efd1d-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="efd1d-105">Действия ASP.NET Core поддерживают передачу одного или нескольких файлов с помощью привязки модели с буферизацией для небольших файлов или потоковой передачи без буферизации для более крупных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="efd1d-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="efd1d-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="efd1d-107">Замечания по безопасности</span><span class="sxs-lookup"><span data-stu-id="efd1d-107">Security considerations</span></span>

<span data-ttu-id="efd1d-108">Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер.</span><span class="sxs-lookup"><span data-stu-id="efd1d-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="efd1d-109">Злоумышленники могут попытаться:</span><span class="sxs-lookup"><span data-stu-id="efd1d-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="efd1d-110">Выполнить атаку типа [отказ в обслуживании](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="efd1d-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="efd1d-111">Передать вирусы и вредоносные программы.</span><span class="sxs-lookup"><span data-stu-id="efd1d-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="efd1d-112">Нарушить безопасность сетей и серверов другими способами.</span><span class="sxs-lookup"><span data-stu-id="efd1d-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="efd1d-113">Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак.</span><span class="sxs-lookup"><span data-stu-id="efd1d-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="efd1d-114">Передавайте файлы в выделенную область для отправки файлов, желательно не на системный диск.</span><span class="sxs-lookup"><span data-stu-id="efd1d-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="efd1d-115">Использование выделенного расположения упрощает применение мер безопасности к отправленным файлам.</span><span class="sxs-lookup"><span data-stu-id="efd1d-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="efd1d-116">Отключите разрешения на выполнение для расположения отправки файла.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="efd1d-117">**Не** сохраняйте переданные файлы в дереве каталогов, где находится приложение.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="efd1d-118">Используйте безопасное имя файла, определяемое приложением.</span><span class="sxs-lookup"><span data-stu-id="efd1d-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="efd1d-119">Не используйте имя файла, предоставленное пользователем, или ненадежное имя переданного файла.&dagger; Кодируйте в формате HTML ненадежное имя файла при его отображении.</span><span class="sxs-lookup"><span data-stu-id="efd1d-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="efd1d-120">Например, при записи имени файла в журнал или отображении в пользовательском интерфейсе (Razor автоматически кодирует выходные данные в формате HTML).</span><span class="sxs-lookup"><span data-stu-id="efd1d-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="efd1d-121">Разрешите только утвержденные расширения файлов для спецификации на проектирование приложения.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="efd1d-122">Убедитесь, что проверки на стороне клиента выполняются и на сервере.&dagger; Проверки на стороне клиента можно легко обойти.</span><span class="sxs-lookup"><span data-stu-id="efd1d-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="efd1d-123">Проверьте размер отправленного файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="efd1d-124">Установите максимальный предельный размер, чтобы предотвратить передачу больших объемов данных.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="efd1d-125">Если файлы не должны перезаписываться переданным файлом с тем же именем, перед отправкой файла проверьте его имя в базе данных или физическом хранилище.</span><span class="sxs-lookup"><span data-stu-id="efd1d-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="efd1d-126">**Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ, прежде чем сохранять файл.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="efd1d-127">&dagger;Пример приложения демонстрирует подход, который соответствует критериям.</span><span class="sxs-lookup"><span data-stu-id="efd1d-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-128">Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:</span><span class="sxs-lookup"><span data-stu-id="efd1d-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="efd1d-129">полностью получить контроль над системой;</span><span class="sxs-lookup"><span data-stu-id="efd1d-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="efd1d-130">перезагрузить систему так, что она окажется в неработоспособном состоянии;</span><span class="sxs-lookup"><span data-stu-id="efd1d-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="efd1d-131">скомпрометировать пользовательские или системные данные;</span><span class="sxs-lookup"><span data-stu-id="efd1d-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="efd1d-132">применить граффити к открытому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="efd1d-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="efd1d-133">Сведения об уменьшении контактной зоны атаки во время приема файлов от пользователей см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="efd1d-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="efd1d-134">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="efd1d-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="efd1d-135">[Безопасность в Azure. Убедитесь, что при приеме файлов от пользователей обеспечиваются соответствующие меры безопасности](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users).</span><span class="sxs-lookup"><span data-stu-id="efd1d-135">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="efd1d-136">Дополнительные сведения о реализации мер безопасности, включая примеры из примера приложения, см. в статье [Передача файлов в ASP.NET Core](#validation).</span><span class="sxs-lookup"><span data-stu-id="efd1d-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="efd1d-137">Сценарии использования хранилища</span><span class="sxs-lookup"><span data-stu-id="efd1d-137">Storage scenarios</span></span>

<span data-ttu-id="efd1d-138">К общим вариантам хранилища файлов относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="efd1d-138">Common storage options for files include:</span></span>

* <span data-ttu-id="efd1d-139">База данных</span><span class="sxs-lookup"><span data-stu-id="efd1d-139">Database</span></span>

  * <span data-ttu-id="efd1d-140">В случае отправки небольших файлов база данных часто работает быстрее, чем физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="efd1d-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="efd1d-141">База данных часто более удобна по сравнению с вариантами физического хранилища, так как получение записи из базы пользовательских данных может одновременно предоставить содержимое файла (например, изображение аватара).</span><span class="sxs-lookup"><span data-stu-id="efd1d-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="efd1d-142">Эксплуатация базы данных потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="efd1d-143">Физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="efd1d-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="efd1d-144">Обработка передачи больших файлов:</span><span class="sxs-lookup"><span data-stu-id="efd1d-144">For large file uploads:</span></span>
    * <span data-ttu-id="efd1d-145">Ограничения базы данных могут ограничивать размер передачи.</span><span class="sxs-lookup"><span data-stu-id="efd1d-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="efd1d-146">Физическое хранилище часто менее экономически выгодно, чем хранилище в базе данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="efd1d-147">Эксплуатация физического хранилища потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="efd1d-148">Процесс приложения должен иметь разрешения на чтение и запись для места хранения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="efd1d-149">**Никогда не предоставляйте разрешение на выполнение.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="efd1d-150">Служба хранилища данных (например, хранилище [BLOB-объектов Azure](https://azure.microsoft.com/services/storage/blobs/)).</span><span class="sxs-lookup"><span data-stu-id="efd1d-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="efd1d-151">Обычно службы обеспечивают улучшенную масштабируемость и устойчивость по сравнению с локальными решениями, которые обычно подвержены единым точкам отказа.</span><span class="sxs-lookup"><span data-stu-id="efd1d-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="efd1d-152">Затраты на использование служб обычно ниже в сценариях с крупномасштабной инфраструктурой хранения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="efd1d-153">Дополнительные сведения см. в разделе [Краткое руководство. Использование .NET для создания большого двоичного объекта в хранилище объектов](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="efd1d-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="efd1d-154">В этом разделе показан метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, но метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> можно использовать для сохранения <xref:System.IO.FileStream> в хранилище BLOB-объектов при работе с <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="efd1d-155">Сценарии передачи файлов</span><span class="sxs-lookup"><span data-stu-id="efd1d-155">File upload scenarios</span></span>

<span data-ttu-id="efd1d-156">Есть два распространенных подхода к передаче файлов — буферизация и потоковая передача.</span><span class="sxs-lookup"><span data-stu-id="efd1d-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="efd1d-157">**Буферизация**</span><span class="sxs-lookup"><span data-stu-id="efd1d-157">**Buffering**</span></span>

<span data-ttu-id="efd1d-158">Весь файл считывается в <xref:Microsoft.AspNetCore.Http.IFormFile> (представление файла на C#, используемого для обработки или сохранения файла).</span><span class="sxs-lookup"><span data-stu-id="efd1d-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="efd1d-159">Потребление ресурсов (диска, памяти) при передаче файлов зависит от количества и размера одновременно передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="efd1d-160">При попытке приложения поместить в буфер слишком много файлов может произойти аварийное завершение работы сайта из-за нехватки памяти или места на диске.</span><span class="sxs-lookup"><span data-stu-id="efd1d-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="efd1d-161">Если размер или частота отправки файлов исчерпывают ресурсы приложения, используйте потоковую передачу.</span><span class="sxs-lookup"><span data-stu-id="efd1d-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="efd1d-162">Один буферизованный файл размером свыше 64 КБ перемещается из памяти во временный файл на диске.</span><span class="sxs-lookup"><span data-stu-id="efd1d-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="efd1d-163">Буферизация небольших файлов описана в следующих разделах этой статьи:</span><span class="sxs-lookup"><span data-stu-id="efd1d-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="efd1d-164">Физическое хранилище</span><span class="sxs-lookup"><span data-stu-id="efd1d-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="efd1d-165">База данных</span><span class="sxs-lookup"><span data-stu-id="efd1d-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="efd1d-166">**Потоковая передача**</span><span class="sxs-lookup"><span data-stu-id="efd1d-166">**Streaming**</span></span>

<span data-ttu-id="efd1d-167">Файл можно получить с помощью составного запроса. Затем он обрабатывается или сохраняется приложением напрямую.</span><span class="sxs-lookup"><span data-stu-id="efd1d-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="efd1d-168">Потоковая передача повышает производительность не значительно.</span><span class="sxs-lookup"><span data-stu-id="efd1d-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="efd1d-169">При отправке файлов потоковая передача снижает нагрузку на память или на место на диске.</span><span class="sxs-lookup"><span data-stu-id="efd1d-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="efd1d-170">Потоковая передача больших файлов рассматривается в разделе [Передача больших файлов с помощью потоковой передачи](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="efd1d-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="efd1d-171">Передача небольших файлов с привязкой буферизованной модели к физическому хранилищу</span><span class="sxs-lookup"><span data-stu-id="efd1d-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="efd1d-172">Для передачи небольших файлов можно применить составную форму или сформировать запрос POST на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="efd1d-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="efd1d-173">В следующем примере демонстрируется использование формы Razor Pages для передачи одного файла (*Pages/BufferedSingleFileUploadPhysical.cshtml* в примере приложения):</span><span class="sxs-lookup"><span data-stu-id="efd1d-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="efd1d-174">Следующий пример аналогичен предыдущему примеру, за исключением следующего:</span><span class="sxs-lookup"><span data-stu-id="efd1d-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="efd1d-175">JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) используется для отправки данных формы.</span><span class="sxs-lookup"><span data-stu-id="efd1d-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="efd1d-176">Проверка не выполняется.</span><span class="sxs-lookup"><span data-stu-id="efd1d-176">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="efd1d-177">Для выполнения отправки формы в JavaScript для клиентов, которые [не поддерживают Fetch API](https://caniuse.com/#feat=fetch), используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="efd1d-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="efd1d-178">Используйте функцию Fetch Polyfill (например, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="efd1d-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="efd1d-179">Используйте ключевое слово `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="efd1d-180">Например:</span><span class="sxs-lookup"><span data-stu-id="efd1d-180">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="efd1d-181">Для поддержки передачи файлов в HTML-формах должен указываться тип кодировки `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="efd1d-182">Для элемента ввода `files`, поддерживающего отправку нескольких файлов, в элементе `<input>` необходимо указать атрибут `multiple`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="efd1d-183">Доступ к отдельным файлам, переданным на сервер, можно получать посредством [привязки модели](xref:mvc/models/model-binding) с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="efd1d-184">В примере приложения показано несколько отправок буферизованных файлов для баз данных и физических хранилищ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="efd1d-185">**Не** используйте свойство `FileName` объекта <xref:Microsoft.AspNetCore.Http.IFormFile>, кроме как для отображения и ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="efd1d-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="efd1d-186">При отображении или ведении журнала кодируйте имя файла в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="efd1d-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="efd1d-187">Злоумышленник может предоставить имя вредоносного файла, включая полные или относительные пути.</span><span class="sxs-lookup"><span data-stu-id="efd1d-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="efd1d-188">Приложения должны:</span><span class="sxs-lookup"><span data-stu-id="efd1d-188">Applications should:</span></span>
>
> * <span data-ttu-id="efd1d-189">удалить путь из имени файла, указываемого пользователем;</span><span class="sxs-lookup"><span data-stu-id="efd1d-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="efd1d-190">сохранить имя файла, закодированное в формате HTML, откуда был удален путь, для пользовательского интерфейса или ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="efd1d-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="efd1d-191">создать случайное имя файла для хранения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="efd1d-192">Следующий код удаляет путь из имени файла:</span><span class="sxs-lookup"><span data-stu-id="efd1d-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="efd1d-193">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="efd1d-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="efd1d-194">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="efd1d-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="efd1d-195">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="efd1d-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="efd1d-196">Проверка</span><span class="sxs-lookup"><span data-stu-id="efd1d-196">Validation</span></span>](#validation)

<span data-ttu-id="efd1d-197">При отправке файлов с помощью привязки модели и <xref:Microsoft.AspNetCore.Http.IFormFile> метод действия может принимать следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="efd1d-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="efd1d-198">Один файл <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="efd1d-199">Любая из следующих коллекций, представляющих несколько файлов:</span><span class="sxs-lookup"><span data-stu-id="efd1d-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="efd1d-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="efd1d-201">Привязка сопоставляет файлы форм по имени.</span><span class="sxs-lookup"><span data-stu-id="efd1d-201">Binding matches form files by name.</span></span> <span data-ttu-id="efd1d-202">Например, значение HTML `name` в `<input type="file" name="formFile">` должно соответствовать привязанному к C# параметру или свойству (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="efd1d-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="efd1d-203">Дополнительные сведения см. в разделе [Сопоставление значения атрибута имени и имени параметра метода POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="efd1d-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="efd1d-204">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="efd1d-204">The following example:</span></span>

* <span data-ttu-id="efd1d-205">Циклично отправляет один или несколько передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="efd1d-206">Использует метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы вернуть полный путь к файлу, включая его имя.</span><span class="sxs-lookup"><span data-stu-id="efd1d-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="efd1d-207">Сохраняет файлы в локальную файловую систему, используя имя файла, созданное приложением.</span><span class="sxs-lookup"><span data-stu-id="efd1d-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="efd1d-208">Возвращает общее число и размер отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-208">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="efd1d-209">Чтобы создать имя файла без пути, используйте `Path.GetRandomFileName`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="efd1d-210">В следующем примере путь получен из конфигурации:</span><span class="sxs-lookup"><span data-stu-id="efd1d-210">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="efd1d-211">Передаваемый в <xref:System.IO.FileStream> путь *должен* содержать имя файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="efd1d-212">Если имя файла не указано, в среде выполнения возникает исключение <xref:System.UnauthorizedAccessException>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="efd1d-213">Файлы, передаваемые с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>, буферизуются в памяти или на диске на сервере перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="efd1d-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="efd1d-214">Внутри метода действия содержимое <xref:Microsoft.AspNetCore.Http.IFormFile> доступно в виде <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="efd1d-215">Помимо локальной файловой системы, файлы можно сохранять в сетевой папке или в службе хранилища файлов, например в хранилище [BLOB-объектов Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="efd1d-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="efd1d-216">Еще один пример, который перебирает несколько файлов для отправки и использует надежные имена файлов, см. в файле *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-217">Метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) вызывает исключение <xref:System.IO.IOException> в случае создания более чем 65 535 файлов без удаления предыдущих временных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="efd1d-218">Ограничение в 65 535 файлов предусмотрено для каждого сервера.</span><span class="sxs-lookup"><span data-stu-id="efd1d-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="efd1d-219">Дополнительные сведения об этом ограничении в ОС Windows см. в примечаниях в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="efd1d-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * <span data-ttu-id="efd1d-220">[Функция GetTempFileNameA](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks);</span><span class="sxs-lookup"><span data-stu-id="efd1d-220">[GetTempFileNameA function](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)</span></span>
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="efd1d-221">Передача небольших файлов с привязкой буферизованной модели к базе данных</span><span class="sxs-lookup"><span data-stu-id="efd1d-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="efd1d-222">Для сохранения данных двоичных файлов в базе данных с помощью [Entity Framework](/ef/core/index) определите для сущности свойство массива <xref:System.Byte>:</span><span class="sxs-lookup"><span data-stu-id="efd1d-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="efd1d-223">Укажите свойство модели страницы для класса, который содержит <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="efd1d-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="efd1d-224"><xref:Microsoft.AspNetCore.Http.IFormFile> можно использовать непосредственно как параметр метода действия или свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="efd1d-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="efd1d-225">В предыдущем примере используется свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="efd1d-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="efd1d-226">В форме Razor Pages используется `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-226">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="efd1d-227">При публикации формы на сервере скопируйте <xref:Microsoft.AspNetCore.Http.IFormFile> в поток и сохраните его в базе данных в виде массива байтов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="efd1d-228">В следующем примере `_dbContext` сохраняет контекст базы данных приложения:</span><span class="sxs-lookup"><span data-stu-id="efd1d-228">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="efd1d-229">Предыдущий пример похож на сценарий, продемонстрированный в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="efd1d-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="efd1d-230">*Pages/BufferedSingleFileUploadDb.cshtml*;</span><span class="sxs-lookup"><span data-stu-id="efd1d-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="efd1d-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="efd1d-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-232">При сохранении двоичных данных в реляционных базах данных следует соблюдать осторожность, так как это может отрицательно сказаться на производительности.</span><span class="sxs-lookup"><span data-stu-id="efd1d-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="efd1d-233">Свойство `FileName` параметра <xref:Microsoft.AspNetCore.Http.IFormFile> требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="efd1d-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="efd1d-234">Свойство `FileName` следует использовать только в целях вывода и только после HTML-кодирования.</span><span class="sxs-lookup"><span data-stu-id="efd1d-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="efd1d-235">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="efd1d-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="efd1d-236">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="efd1d-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="efd1d-237">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="efd1d-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="efd1d-238">Проверка</span><span class="sxs-lookup"><span data-stu-id="efd1d-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="efd1d-239">Передача больших файлов с помощью потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="efd1d-239">Upload large files with streaming</span></span>

<span data-ttu-id="efd1d-240">В приведенном ниже примере демонстрируется использование JavaScript для потоковой передачи файла в действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="efd1d-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="efd1d-241">Токен против подделки файла создается с помощью пользовательского атрибута фильтра и передается в заголовках HTTP клиента, а не в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="efd1d-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="efd1d-242">Так как метод действия обрабатывает передаваемые данные напрямую, привязка модели формы отключается другим пользовательским фильтром.</span><span class="sxs-lookup"><span data-stu-id="efd1d-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="efd1d-243">Внутри действия содержимое формы считывается с помощью объекта `MultipartReader`, который считывает каждый объект `MultipartSection` по отдельности, обрабатывая файл или сохраняя содержимое.</span><span class="sxs-lookup"><span data-stu-id="efd1d-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="efd1d-244">После считывания составных разделов действие выполняет собственную привязку модели.</span><span class="sxs-lookup"><span data-stu-id="efd1d-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="efd1d-245">Вначале страница загружает форму и сохраняет токен против подделки в файле cookie (с помощью атрибута `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="efd1d-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="efd1d-246">Атрибут использует встроенную поддержку [защиты от подделки](xref:security/anti-request-forgery) в ASP.NET Core, чтобы задать файл cookie с токеном запроса.</span><span class="sxs-lookup"><span data-stu-id="efd1d-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="efd1d-247">Для отключения привязки модели используется `DisableFormValueModelBindingAttribute`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="efd1d-248">В примере приложения `GenerateAntiforgeryTokenCookieAttribute` и `DisableFormValueModelBindingAttribute` применяются в качестве фильтров к моделям приложений на странице `/StreamedSingleFileUploadDb` и `/StreamedSingleFileUploadPhysical` в `Startup.ConfigureServices` с использованием [соглашений Razor Pages](xref:razor-pages/razor-pages-conventions).</span><span class="sxs-lookup"><span data-stu-id="efd1d-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="efd1d-249">Так как привязка модели не считывает форму, параметры, привязанные из формы, не привязываются (запросы, маршруты и заголовки продолжают работать).</span><span class="sxs-lookup"><span data-stu-id="efd1d-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="efd1d-250">Метод действия работает напрямую со свойством `Request`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="efd1d-251">Для считывания каждого раздела служит объект `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="efd1d-252">Данные "ключ — значение" хранятся в `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="efd1d-253">После считывания составных разделов содержимое `KeyValueAccumulator` используется для привязки данных формы к типу модели.</span><span class="sxs-lookup"><span data-stu-id="efd1d-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="efd1d-254">Полный метод `StreamingController.UploadDatabase` для потоковой передачи в базу данных с EF Core:</span><span class="sxs-lookup"><span data-stu-id="efd1d-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="efd1d-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="efd1d-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="efd1d-256">Полный метод `StreamingController.UploadPhysical` для потоковой передачи в физическое расположение:</span><span class="sxs-lookup"><span data-stu-id="efd1d-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="efd1d-257">В примере приложения проверки обрабатываются с помощью `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="efd1d-258">Проверка</span><span class="sxs-lookup"><span data-stu-id="efd1d-258">Validation</span></span>

<span data-ttu-id="efd1d-259">В классе `FileHelpers` примера приложения демонстрируется несколько проверок буферизованного файла <xref:Microsoft.AspNetCore.Http.IFormFile> и потоковой передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="efd1d-260">Сведения об обработке буферизованных файлов <xref:Microsoft.AspNetCore.Http.IFormFile> в примере приложения см. в описании метода `ProcessFormFile` в файле *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="efd1d-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="efd1d-261">Сведения об обработке потоковых файлов см. в описании метода `ProcessStreamedFile` в том же файле.</span><span class="sxs-lookup"><span data-stu-id="efd1d-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-262">Методы обработки проверки, показанные в примере приложения, не проверяют содержимое отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="efd1d-263">В большинстве рабочих сценариев в файле применяется API сканирования на наличие вирусов и вредоносных программ, прежде чем сделать файл доступным для пользователей или других систем.</span><span class="sxs-lookup"><span data-stu-id="efd1d-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="efd1d-264">Хотя пример в разделе содержит рабочий пример методов проверки, не следует реализовывать класс `FileHelpers` в рабочем приложении, кроме таких случаев:</span><span class="sxs-lookup"><span data-stu-id="efd1d-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="efd1d-265">Вы полностью разбираетесь в реализации.</span><span class="sxs-lookup"><span data-stu-id="efd1d-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="efd1d-266">Вы изменяете реализацию соответствующим образом для среды и спецификаций приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="efd1d-267">**Никогда не реализуйте код безопасности в приложении, не выполнив эти требования.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="efd1d-268">Проверка содержимого</span><span class="sxs-lookup"><span data-stu-id="efd1d-268">Content validation</span></span>

<span data-ttu-id="efd1d-269">**Используйте сторонний API сканирования на наличие вирусов и вредоносных программ для отправленного содержимого.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="efd1d-270">Сканирование файлов требует использования ресурсов сервера в сценариях с большими объемами данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="efd1d-271">Если производительность обработки запросов снижается из-за сканирования файлов, рассмотрите возможность разгрузки сканирования путем использования [фоновой службы](xref:fundamentals/host/hosted-services), возможно, службы, которая работает на сервере, отличном от сервера приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="efd1d-272">Как правило, передаваемые файлы хранятся в карантинной области до тех пор, пока фоновый сканер не проверит их на наличие вирусов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="efd1d-273">При передаче файл перемещается в нормальное место хранения файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="efd1d-274">Эти действия обычно выполняются вместе с записью базы данных, которая указывает состояние сканирования файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="efd1d-275">Благодаря такому подходу приложение и сервер приложений остаются в режиме реагирования на запросы.</span><span class="sxs-lookup"><span data-stu-id="efd1d-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="efd1d-276">Проверка расширения файла</span><span class="sxs-lookup"><span data-stu-id="efd1d-276">File extension validation</span></span>

<span data-ttu-id="efd1d-277">Расширение переданного файла должно проверяться в соответствии со списком разрешенных расширений.</span><span class="sxs-lookup"><span data-stu-id="efd1d-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="efd1d-278">Например:</span><span class="sxs-lookup"><span data-stu-id="efd1d-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="efd1d-279">Проверка подписи файла</span><span class="sxs-lookup"><span data-stu-id="efd1d-279">File signature validation</span></span>

<span data-ttu-id="efd1d-280">Подпись файла определяется по первым нескольким байтам в начале файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="efd1d-281">Эти байты можно использовать, чтобы указать, совпадает ли расширение с содержимым файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="efd1d-282">Пример приложения проверяет подписи файлов на соответствие нескольким распространенным типам файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="efd1d-283">В следующем примере проверяется подпись файла для изображения JPEG:</span><span class="sxs-lookup"><span data-stu-id="efd1d-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="efd1d-284">Дополнительные сведения о получении дополнительных подписей файлов см. [по этой ссылке](https://www.filesignatures.net/) и в официальных спецификациях файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="efd1d-285">Безопасность имени файла</span><span class="sxs-lookup"><span data-stu-id="efd1d-285">File name security</span></span>

<span data-ttu-id="efd1d-286">Никогда не используйте для хранения файла в физическом хранилище имя, предоставляемое клиентом.</span><span class="sxs-lookup"><span data-stu-id="efd1d-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="efd1d-287">Создайте надежное имя файла с помощью [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) или [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы получить полный путь (включая имя файла) для временного хранилища.</span><span class="sxs-lookup"><span data-stu-id="efd1d-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="efd1d-288">Razor автоматически кодирует в HTML значения свойств для вывода.</span><span class="sxs-lookup"><span data-stu-id="efd1d-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="efd1d-289">Ниже приведен безопасный код, который можно использовать.</span><span class="sxs-lookup"><span data-stu-id="efd1d-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="efd1d-290">Вне Razor содержимое имени файла из запроса пользователя всегда кодируется в <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="efd1d-291">Во многих реализациях следует включать проверку существования файла. В противном случае файл перезаписывается файлом с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="efd1d-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="efd1d-292">Предоставьте дополнительную логику для соответствия спецификациям приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="efd1d-293">Проверка размера</span><span class="sxs-lookup"><span data-stu-id="efd1d-293">Size validation</span></span>

<span data-ttu-id="efd1d-294">Ограничьте размер передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="efd1d-295">В примере приложения размер файла ограничен 2 МБ (указывается в байтах).</span><span class="sxs-lookup"><span data-stu-id="efd1d-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="efd1d-296">Ограничение предоставляется с помощью [конфигурации](xref:fundamentals/configuration/index) из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="efd1d-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="efd1d-297">В классы `PageModel` внедряется `FileSizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="efd1d-298">Если размер файла превышает ограничение, файл отклоняется:</span><span class="sxs-lookup"><span data-stu-id="efd1d-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="efd1d-299">Сопоставление значения атрибута имени и имени параметра метода POST</span><span class="sxs-lookup"><span data-stu-id="efd1d-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="efd1d-300">В формах, не относящихся к Razor, которые передают данные формы или используют `FormData` JavaScript напрямую, имя, указанное в элементе формы или `FormData`, должно соответствовать имени параметра в действии контроллера.</span><span class="sxs-lookup"><span data-stu-id="efd1d-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="efd1d-301">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="efd1d-301">In the following example:</span></span>

* <span data-ttu-id="efd1d-302">При использовании элемента `<input>` атрибуту `name` присваивается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="efd1d-303">При использовании `FormData` в JavaScript для имени задается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="efd1d-304">Используйте соответствующее имя для параметра метода C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="efd1d-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="efd1d-305">Для метода обработчика страницы Razor Pages с именем `Upload`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="efd1d-306">Для метода действия контроллера POST приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="efd1d-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="efd1d-307">Конфигурация сервера и приложения</span><span class="sxs-lookup"><span data-stu-id="efd1d-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="efd1d-308">Ограничение длины составного текста</span><span class="sxs-lookup"><span data-stu-id="efd1d-308">Multipart body length limit</span></span>

<span data-ttu-id="efd1d-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> устанавливает ограничение длины каждого составного текста.</span><span class="sxs-lookup"><span data-stu-id="efd1d-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="efd1d-310">Разделы формы, превышающие это ограничение, вызовут <xref:System.IO.InvalidDataException> при синтаксическом анализе.</span><span class="sxs-lookup"><span data-stu-id="efd1d-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="efd1d-311">Значение по умолчанию — 134 217 728 байт (128 МБ).</span><span class="sxs-lookup"><span data-stu-id="efd1d-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="efd1d-312">Настройте ограничение с помощью параметра <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="efd1d-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> используется для настройки <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="efd1d-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="efd1d-314">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

<span data-ttu-id="efd1d-315">В приложении Razor Pages или MVC примените фильтр к модели страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="efd1d-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="efd1d-316">Максимальный размер текста запроса Kestrel</span><span class="sxs-lookup"><span data-stu-id="efd1d-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="efd1d-317">Для приложений, размещенных в Kestrel, по умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="efd1d-318">Настройте ограничение с помощью параметра сервера Kestrel [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size):</span><span class="sxs-lookup"><span data-stu-id="efd1d-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="efd1d-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> используется для настройки [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="efd1d-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="efd1d-320">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

<span data-ttu-id="efd1d-321">В приложении Razor Pages или MVC примените фильтр к классу обработчика страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="efd1d-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="efd1d-322">`RequestSizeLimitAttribute` можно также применить с помощью директивы Razor [@attribute](xref:mvc/views/razor#attribute).</span><span class="sxs-lookup"><span data-stu-id="efd1d-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="efd1d-323">Другие ограничения Kestrel</span><span class="sxs-lookup"><span data-stu-id="efd1d-323">Other Kestrel limits</span></span>

<span data-ttu-id="efd1d-324">Другие ограничения Kestrel могут применяться для приложений, размещенных в Kestrel:</span><span class="sxs-lookup"><span data-stu-id="efd1d-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="efd1d-325">Максимальное число клиентских подключений</span><span class="sxs-lookup"><span data-stu-id="efd1d-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="efd1d-326">Скорость передачи данных запросов и ответов</span><span class="sxs-lookup"><span data-stu-id="efd1d-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="efd1d-327">Ограничение длины содержимого IIS</span><span class="sxs-lookup"><span data-stu-id="efd1d-327">IIS content length limit</span></span>

<span data-ttu-id="efd1d-328">По умолчанию ограничение запроса (`maxAllowedContentLength`) составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="efd1d-329">Ограничение можно настроить в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="efd1d-329">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="efd1d-330">Этот параметр применим только к службам IIS.</span><span class="sxs-lookup"><span data-stu-id="efd1d-330">This setting only applies to IIS.</span></span> <span data-ttu-id="efd1d-331">При размещении в Kestrel такой проблемы возникать не должно.</span><span class="sxs-lookup"><span data-stu-id="efd1d-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="efd1d-332">Дополнительные сведения см. в статье [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Ограничения запроса <requestLimits>).</span><span class="sxs-lookup"><span data-stu-id="efd1d-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="efd1d-333">Ограничения в модуле ASP.NET Core или наличие модуля фильтрации запросов IIS могут ограничить передачу до 2 или 4 ГБ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="efd1d-334">Дополнительные сведения см. в [этой ветке форума](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="efd1d-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="efd1d-335">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="efd1d-335">Troubleshoot</span></span>

<span data-ttu-id="efd1d-336">Ниже описываются некоторые распространенные проблемы, которые возникают при передаче файлов, и возможные способы их решения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="efd1d-337">Ошибка "Не найдено" при развертывании на сервере IIS</span><span class="sxs-lookup"><span data-stu-id="efd1d-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="efd1d-338">Следующая ошибка свидетельствует о том, что размер передаваемых файлов превышает настроенное на сервере ограничение длины содержимого:</span><span class="sxs-lookup"><span data-stu-id="efd1d-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="efd1d-339">Дополнительные сведения об увеличении предела см. в разделе [Ограничение длины содержимого IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="efd1d-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="efd1d-340">Сбой подключения</span><span class="sxs-lookup"><span data-stu-id="efd1d-340">Connection failure</span></span>

<span data-ttu-id="efd1d-341">Ошибка соединения и сброс соединения с сервером, скорее всего, означают, что размер отправленного файла превышает максимальную длину текста запроса Kestrel.</span><span class="sxs-lookup"><span data-stu-id="efd1d-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="efd1d-342">Дополнительные сведения см. в разделе [Максимальный размер текста запроса Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="efd1d-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="efd1d-343">Может также потребоваться настроить ограничения подключений клиентов Kestrel.</span><span class="sxs-lookup"><span data-stu-id="efd1d-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="efd1d-344">Исключение, связанное с пустой ссылкой в IFormFile</span><span class="sxs-lookup"><span data-stu-id="efd1d-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="efd1d-345">Если контроллер принимает передаваемые файлы с помощью <xref:Microsoft.AspNetCore.Http.IFormFile>, но значение равно `null`, проверьте, указан ли в HTML-форме атрибут `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="efd1d-346">Если этот атрибут не задан для элемента `<form>`, передача файлов происходить не будет и все связанные аргументы <xref:Microsoft.AspNetCore.Http.IFormFile> будут иметь значение `null`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="efd1d-347">Убедитесь также, что [именование передачи в данных формы совпадает с именованием приложения](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="efd1d-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="efd1d-348">Действия ASP.NET Core поддерживают передачу одного или нескольких файлов с помощью привязки модели с буферизацией для небольших файлов или потоковой передачи без буферизации для более крупных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-348">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="efd1d-349">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="efd1d-349">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="efd1d-350">Замечания по безопасности</span><span class="sxs-lookup"><span data-stu-id="efd1d-350">Security considerations</span></span>

<span data-ttu-id="efd1d-351">Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер.</span><span class="sxs-lookup"><span data-stu-id="efd1d-351">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="efd1d-352">Злоумышленники могут попытаться:</span><span class="sxs-lookup"><span data-stu-id="efd1d-352">Attackers may attempt to:</span></span>

* <span data-ttu-id="efd1d-353">Выполнить атаку типа [отказ в обслуживании](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="efd1d-353">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="efd1d-354">Передать вирусы и вредоносные программы.</span><span class="sxs-lookup"><span data-stu-id="efd1d-354">Upload viruses or malware.</span></span>
* <span data-ttu-id="efd1d-355">Нарушить безопасность сетей и серверов другими способами.</span><span class="sxs-lookup"><span data-stu-id="efd1d-355">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="efd1d-356">Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак.</span><span class="sxs-lookup"><span data-stu-id="efd1d-356">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="efd1d-357">Передавайте файлы в выделенную область для отправки файлов, желательно не на системный диск.</span><span class="sxs-lookup"><span data-stu-id="efd1d-357">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="efd1d-358">Использование выделенного расположения упрощает применение мер безопасности к отправленным файлам.</span><span class="sxs-lookup"><span data-stu-id="efd1d-358">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="efd1d-359">Отключите разрешения на выполнение для расположения отправки файла.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-359">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="efd1d-360">**Не** сохраняйте переданные файлы в дереве каталогов, где находится приложение.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-360">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="efd1d-361">Используйте безопасное имя файла, определяемое приложением.</span><span class="sxs-lookup"><span data-stu-id="efd1d-361">Use a safe file name determined by the app.</span></span> <span data-ttu-id="efd1d-362">Не используйте имя файла, предоставленное пользователем, или ненадежное имя переданного файла.&dagger; Кодируйте в формате HTML ненадежное имя файла при его отображении.</span><span class="sxs-lookup"><span data-stu-id="efd1d-362">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="efd1d-363">Например, при записи имени файла в журнал или отображении в пользовательском интерфейсе (Razor автоматически кодирует выходные данные в формате HTML).</span><span class="sxs-lookup"><span data-stu-id="efd1d-363">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="efd1d-364">Разрешите только утвержденные расширения файлов для спецификации на проектирование приложения.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-364">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="efd1d-365">Убедитесь, что проверки на стороне клиента выполняются и на сервере.&dagger; Проверки на стороне клиента можно легко обойти.</span><span class="sxs-lookup"><span data-stu-id="efd1d-365">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="efd1d-366">Проверьте размер отправленного файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-366">Check the size of an uploaded file.</span></span> <span data-ttu-id="efd1d-367">Установите максимальный предельный размер, чтобы предотвратить передачу больших объемов данных.&dagger;</span><span class="sxs-lookup"><span data-stu-id="efd1d-367">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="efd1d-368">Если файлы не должны перезаписываться переданным файлом с тем же именем, перед отправкой файла проверьте его имя в базе данных или физическом хранилище.</span><span class="sxs-lookup"><span data-stu-id="efd1d-368">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="efd1d-369">**Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ, прежде чем сохранять файл.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-369">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="efd1d-370">&dagger;Пример приложения демонстрирует подход, который соответствует критериям.</span><span class="sxs-lookup"><span data-stu-id="efd1d-370">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-371">Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:</span><span class="sxs-lookup"><span data-stu-id="efd1d-371">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="efd1d-372">полностью получить контроль над системой;</span><span class="sxs-lookup"><span data-stu-id="efd1d-372">Completely gain control of a system.</span></span>
> * <span data-ttu-id="efd1d-373">перезагрузить систему так, что она окажется в неработоспособном состоянии;</span><span class="sxs-lookup"><span data-stu-id="efd1d-373">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="efd1d-374">скомпрометировать пользовательские или системные данные;</span><span class="sxs-lookup"><span data-stu-id="efd1d-374">Compromise user or system data.</span></span>
> * <span data-ttu-id="efd1d-375">применить граффити к открытому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="efd1d-375">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="efd1d-376">Сведения об уменьшении контактной зоны атаки во время приема файлов от пользователей см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="efd1d-376">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="efd1d-377">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="efd1d-377">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="efd1d-378">[Безопасность в Azure. Убедитесь, что при приеме файлов от пользователей обеспечиваются соответствующие меры безопасности](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users).</span><span class="sxs-lookup"><span data-stu-id="efd1d-378">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="efd1d-379">Дополнительные сведения о реализации мер безопасности, включая примеры из примера приложения, см. в статье [Передача файлов в ASP.NET Core](#validation).</span><span class="sxs-lookup"><span data-stu-id="efd1d-379">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="efd1d-380">Сценарии использования хранилища</span><span class="sxs-lookup"><span data-stu-id="efd1d-380">Storage scenarios</span></span>

<span data-ttu-id="efd1d-381">К общим вариантам хранилища файлов относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="efd1d-381">Common storage options for files include:</span></span>

* <span data-ttu-id="efd1d-382">База данных</span><span class="sxs-lookup"><span data-stu-id="efd1d-382">Database</span></span>

  * <span data-ttu-id="efd1d-383">В случае отправки небольших файлов база данных часто работает быстрее, чем физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="efd1d-383">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="efd1d-384">База данных часто более удобна по сравнению с вариантами физического хранилища, так как получение записи из базы пользовательских данных может одновременно предоставить содержимое файла (например, изображение аватара).</span><span class="sxs-lookup"><span data-stu-id="efd1d-384">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="efd1d-385">Эксплуатация базы данных потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-385">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="efd1d-386">Физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="efd1d-386">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="efd1d-387">Обработка передачи больших файлов:</span><span class="sxs-lookup"><span data-stu-id="efd1d-387">For large file uploads:</span></span>
    * <span data-ttu-id="efd1d-388">Ограничения базы данных могут ограничивать размер передачи.</span><span class="sxs-lookup"><span data-stu-id="efd1d-388">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="efd1d-389">Физическое хранилище часто менее экономически выгодно, чем хранилище в базе данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-389">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="efd1d-390">Эксплуатация физического хранилища потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-390">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="efd1d-391">Процесс приложения должен иметь разрешения на чтение и запись для места хранения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-391">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="efd1d-392">**Никогда не предоставляйте разрешение на выполнение.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-392">**Never grant execute permission.**</span></span>

* <span data-ttu-id="efd1d-393">Служба хранилища данных (например, хранилище [BLOB-объектов Azure](https://azure.microsoft.com/services/storage/blobs/)).</span><span class="sxs-lookup"><span data-stu-id="efd1d-393">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="efd1d-394">Обычно службы обеспечивают улучшенную масштабируемость и устойчивость по сравнению с локальными решениями, которые обычно подвержены единым точкам отказа.</span><span class="sxs-lookup"><span data-stu-id="efd1d-394">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="efd1d-395">Затраты на использование служб обычно ниже в сценариях с крупномасштабной инфраструктурой хранения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-395">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="efd1d-396">Дополнительные сведения см. в разделе [Краткое руководство. Использование .NET для создания большого двоичного объекта в хранилище объектов](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="efd1d-396">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="efd1d-397">В этом разделе показан метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, но метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> можно использовать для сохранения <xref:System.IO.FileStream> в хранилище BLOB-объектов при работе с <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-397">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="efd1d-398">Сценарии передачи файлов</span><span class="sxs-lookup"><span data-stu-id="efd1d-398">File upload scenarios</span></span>

<span data-ttu-id="efd1d-399">Есть два распространенных подхода к передаче файлов — буферизация и потоковая передача.</span><span class="sxs-lookup"><span data-stu-id="efd1d-399">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="efd1d-400">**Буферизация**</span><span class="sxs-lookup"><span data-stu-id="efd1d-400">**Buffering**</span></span>

<span data-ttu-id="efd1d-401">Весь файл считывается в <xref:Microsoft.AspNetCore.Http.IFormFile> (представление файла на C#, используемого для обработки или сохранения файла).</span><span class="sxs-lookup"><span data-stu-id="efd1d-401">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="efd1d-402">Потребление ресурсов (диска, памяти) при передаче файлов зависит от количества и размера одновременно передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-402">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="efd1d-403">При попытке приложения поместить в буфер слишком много файлов может произойти аварийное завершение работы сайта из-за нехватки памяти или места на диске.</span><span class="sxs-lookup"><span data-stu-id="efd1d-403">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="efd1d-404">Если размер или частота отправки файлов исчерпывают ресурсы приложения, используйте потоковую передачу.</span><span class="sxs-lookup"><span data-stu-id="efd1d-404">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="efd1d-405">Один буферизованный файл размером свыше 64 КБ перемещается из памяти во временный файл на диске.</span><span class="sxs-lookup"><span data-stu-id="efd1d-405">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="efd1d-406">Буферизация небольших файлов описана в следующих разделах этой статьи:</span><span class="sxs-lookup"><span data-stu-id="efd1d-406">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="efd1d-407">Физическое хранилище</span><span class="sxs-lookup"><span data-stu-id="efd1d-407">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="efd1d-408">База данных</span><span class="sxs-lookup"><span data-stu-id="efd1d-408">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="efd1d-409">**Потоковая передача**</span><span class="sxs-lookup"><span data-stu-id="efd1d-409">**Streaming**</span></span>

<span data-ttu-id="efd1d-410">Файл можно получить с помощью составного запроса. Затем он обрабатывается или сохраняется приложением напрямую.</span><span class="sxs-lookup"><span data-stu-id="efd1d-410">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="efd1d-411">Потоковая передача повышает производительность не значительно.</span><span class="sxs-lookup"><span data-stu-id="efd1d-411">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="efd1d-412">При отправке файлов потоковая передача снижает нагрузку на память или на место на диске.</span><span class="sxs-lookup"><span data-stu-id="efd1d-412">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="efd1d-413">Потоковая передача больших файлов рассматривается в разделе [Передача больших файлов с помощью потоковой передачи](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="efd1d-413">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="efd1d-414">Передача небольших файлов с привязкой буферизованной модели к физическому хранилищу</span><span class="sxs-lookup"><span data-stu-id="efd1d-414">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="efd1d-415">Для передачи небольших файлов можно применить составную форму или сформировать запрос POST на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="efd1d-415">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="efd1d-416">В следующем примере демонстрируется использование формы Razor Pages для передачи одного файла (*Pages/BufferedSingleFileUploadPhysical.cshtml* в примере приложения):</span><span class="sxs-lookup"><span data-stu-id="efd1d-416">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="efd1d-417">Следующий пример аналогичен предыдущему примеру, за исключением следующего:</span><span class="sxs-lookup"><span data-stu-id="efd1d-417">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="efd1d-418">JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) используется для отправки данных формы.</span><span class="sxs-lookup"><span data-stu-id="efd1d-418">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="efd1d-419">Проверка не выполняется.</span><span class="sxs-lookup"><span data-stu-id="efd1d-419">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="efd1d-420">Для выполнения отправки формы в JavaScript для клиентов, которые [не поддерживают Fetch API](https://caniuse.com/#feat=fetch), используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="efd1d-420">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="efd1d-421">Используйте функцию Fetch Polyfill (например, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="efd1d-421">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="efd1d-422">Используйте ключевое слово `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-422">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="efd1d-423">Например:</span><span class="sxs-lookup"><span data-stu-id="efd1d-423">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="efd1d-424">Для поддержки передачи файлов в HTML-формах должен указываться тип кодировки `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-424">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="efd1d-425">Для элемента ввода `files`, поддерживающего отправку нескольких файлов, в элементе `<input>` необходимо указать атрибут `multiple`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-425">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="efd1d-426">Доступ к отдельным файлам, переданным на сервер, можно получать посредством [привязки модели](xref:mvc/models/model-binding) с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-426">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="efd1d-427">В примере приложения показано несколько отправок буферизованных файлов для баз данных и физических хранилищ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-427">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="efd1d-428">**Не** используйте свойство `FileName` объекта <xref:Microsoft.AspNetCore.Http.IFormFile>, кроме как для отображения и ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="efd1d-428">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="efd1d-429">При отображении или ведении журнала кодируйте имя файла в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="efd1d-429">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="efd1d-430">Злоумышленник может предоставить имя вредоносного файла, включая полные или относительные пути.</span><span class="sxs-lookup"><span data-stu-id="efd1d-430">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="efd1d-431">Приложения должны:</span><span class="sxs-lookup"><span data-stu-id="efd1d-431">Applications should:</span></span>
>
> * <span data-ttu-id="efd1d-432">удалить путь из имени файла, указываемого пользователем;</span><span class="sxs-lookup"><span data-stu-id="efd1d-432">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="efd1d-433">сохранить имя файла, закодированное в формате HTML, откуда был удален путь, для пользовательского интерфейса или ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="efd1d-433">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="efd1d-434">создать случайное имя файла для хранения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-434">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="efd1d-435">Следующий код удаляет путь из имени файла:</span><span class="sxs-lookup"><span data-stu-id="efd1d-435">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="efd1d-436">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="efd1d-436">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="efd1d-437">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="efd1d-437">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="efd1d-438">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="efd1d-438">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="efd1d-439">Проверка</span><span class="sxs-lookup"><span data-stu-id="efd1d-439">Validation</span></span>](#validation)

<span data-ttu-id="efd1d-440">При отправке файлов с помощью привязки модели и <xref:Microsoft.AspNetCore.Http.IFormFile> метод действия может принимать следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="efd1d-440">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="efd1d-441">Один файл <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-441">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="efd1d-442">Любая из следующих коллекций, представляющих несколько файлов:</span><span class="sxs-lookup"><span data-stu-id="efd1d-442">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="efd1d-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="efd1d-444">Привязка сопоставляет файлы форм по имени.</span><span class="sxs-lookup"><span data-stu-id="efd1d-444">Binding matches form files by name.</span></span> <span data-ttu-id="efd1d-445">Например, значение HTML `name` в `<input type="file" name="formFile">` должно соответствовать привязанному к C# параметру или свойству (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="efd1d-445">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="efd1d-446">Дополнительные сведения см. в разделе [Сопоставление значения атрибута имени и имени параметра метода POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="efd1d-446">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="efd1d-447">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="efd1d-447">The following example:</span></span>

* <span data-ttu-id="efd1d-448">Циклично отправляет один или несколько передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-448">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="efd1d-449">Использует метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы вернуть полный путь к файлу, включая его имя.</span><span class="sxs-lookup"><span data-stu-id="efd1d-449">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="efd1d-450">Сохраняет файлы в локальную файловую систему, используя имя файла, созданное приложением.</span><span class="sxs-lookup"><span data-stu-id="efd1d-450">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="efd1d-451">Возвращает общее число и размер отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-451">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="efd1d-452">Чтобы создать имя файла без пути, используйте `Path.GetRandomFileName`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-452">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="efd1d-453">В следующем примере путь получен из конфигурации:</span><span class="sxs-lookup"><span data-stu-id="efd1d-453">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="efd1d-454">Передаваемый в <xref:System.IO.FileStream> путь *должен* содержать имя файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-454">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="efd1d-455">Если имя файла не указано, в среде выполнения возникает исключение <xref:System.UnauthorizedAccessException>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-455">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="efd1d-456">Файлы, передаваемые с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>, буферизуются в памяти или на диске на сервере перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="efd1d-456">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="efd1d-457">Внутри метода действия содержимое <xref:Microsoft.AspNetCore.Http.IFormFile> доступно в виде <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-457">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="efd1d-458">Помимо локальной файловой системы, файлы можно сохранять в сетевой папке или в службе хранилища файлов, например в хранилище [BLOB-объектов Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="efd1d-458">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="efd1d-459">Еще один пример, который перебирает несколько файлов для отправки и использует надежные имена файлов, см. в файле *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-459">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-460">Метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) вызывает исключение <xref:System.IO.IOException> в случае создания более чем 65 535 файлов без удаления предыдущих временных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-460">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="efd1d-461">Ограничение в 65 535 файлов предусмотрено для каждого сервера.</span><span class="sxs-lookup"><span data-stu-id="efd1d-461">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="efd1d-462">Дополнительные сведения об этом ограничении в ОС Windows см. в примечаниях в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="efd1d-462">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * <span data-ttu-id="efd1d-463">[Функция GetTempFileNameA](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks);</span><span class="sxs-lookup"><span data-stu-id="efd1d-463">[GetTempFileNameA function](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)</span></span>
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="efd1d-464">Передача небольших файлов с привязкой буферизованной модели к базе данных</span><span class="sxs-lookup"><span data-stu-id="efd1d-464">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="efd1d-465">Для сохранения данных двоичных файлов в базе данных с помощью [Entity Framework](/ef/core/index) определите для сущности свойство массива <xref:System.Byte>:</span><span class="sxs-lookup"><span data-stu-id="efd1d-465">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="efd1d-466">Укажите свойство модели страницы для класса, который содержит <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="efd1d-466">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="efd1d-467"><xref:Microsoft.AspNetCore.Http.IFormFile> можно использовать непосредственно как параметр метода действия или свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="efd1d-467"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="efd1d-468">В предыдущем примере используется свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="efd1d-468">The prior example uses a bound model property.</span></span>

<span data-ttu-id="efd1d-469">В форме Razor Pages используется `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-469">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="efd1d-470">При публикации формы на сервере скопируйте <xref:Microsoft.AspNetCore.Http.IFormFile> в поток и сохраните его в базе данных в виде массива байтов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-470">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="efd1d-471">В следующем примере `_dbContext` сохраняет контекст базы данных приложения:</span><span class="sxs-lookup"><span data-stu-id="efd1d-471">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="efd1d-472">Предыдущий пример похож на сценарий, продемонстрированный в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="efd1d-472">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="efd1d-473">*Pages/BufferedSingleFileUploadDb.cshtml*;</span><span class="sxs-lookup"><span data-stu-id="efd1d-473">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="efd1d-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="efd1d-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-475">При сохранении двоичных данных в реляционных базах данных следует соблюдать осторожность, так как это может отрицательно сказаться на производительности.</span><span class="sxs-lookup"><span data-stu-id="efd1d-475">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="efd1d-476">Свойство `FileName` параметра <xref:Microsoft.AspNetCore.Http.IFormFile> требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="efd1d-476">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="efd1d-477">Свойство `FileName` следует использовать только в целях вывода и только после HTML-кодирования.</span><span class="sxs-lookup"><span data-stu-id="efd1d-477">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="efd1d-478">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="efd1d-478">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="efd1d-479">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="efd1d-479">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="efd1d-480">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="efd1d-480">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="efd1d-481">Проверка</span><span class="sxs-lookup"><span data-stu-id="efd1d-481">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="efd1d-482">Передача больших файлов с помощью потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="efd1d-482">Upload large files with streaming</span></span>

<span data-ttu-id="efd1d-483">В приведенном ниже примере демонстрируется использование JavaScript для потоковой передачи файла в действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="efd1d-483">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="efd1d-484">Токен против подделки файла создается с помощью пользовательского атрибута фильтра и передается в заголовках HTTP клиента, а не в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="efd1d-484">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="efd1d-485">Так как метод действия обрабатывает передаваемые данные напрямую, привязка модели формы отключается другим пользовательским фильтром.</span><span class="sxs-lookup"><span data-stu-id="efd1d-485">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="efd1d-486">Внутри действия содержимое формы считывается с помощью объекта `MultipartReader`, который считывает каждый объект `MultipartSection` по отдельности, обрабатывая файл или сохраняя содержимое.</span><span class="sxs-lookup"><span data-stu-id="efd1d-486">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="efd1d-487">После считывания составных разделов действие выполняет собственную привязку модели.</span><span class="sxs-lookup"><span data-stu-id="efd1d-487">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="efd1d-488">Вначале страница загружает форму и сохраняет токен против подделки в файле cookie (с помощью атрибута `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="efd1d-488">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="efd1d-489">Атрибут использует встроенную поддержку [защиты от подделки](xref:security/anti-request-forgery) в ASP.NET Core, чтобы задать файл cookie с токеном запроса.</span><span class="sxs-lookup"><span data-stu-id="efd1d-489">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="efd1d-490">Для отключения привязки модели используется `DisableFormValueModelBindingAttribute`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-490">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="efd1d-491">В примере приложения `GenerateAntiforgeryTokenCookieAttribute` и `DisableFormValueModelBindingAttribute` применяются в качестве фильтров к моделям приложений на странице `/StreamedSingleFileUploadDb` и `/StreamedSingleFileUploadPhysical` в `Startup.ConfigureServices` с использованием [соглашений Razor Pages](xref:razor-pages/razor-pages-conventions).</span><span class="sxs-lookup"><span data-stu-id="efd1d-491">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="efd1d-492">Так как привязка модели не считывает форму, параметры, привязанные из формы, не привязываются (запросы, маршруты и заголовки продолжают работать).</span><span class="sxs-lookup"><span data-stu-id="efd1d-492">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="efd1d-493">Метод действия работает напрямую со свойством `Request`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-493">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="efd1d-494">Для считывания каждого раздела служит объект `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-494">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="efd1d-495">Данные "ключ — значение" хранятся в `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-495">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="efd1d-496">После считывания составных разделов содержимое `KeyValueAccumulator` используется для привязки данных формы к типу модели.</span><span class="sxs-lookup"><span data-stu-id="efd1d-496">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="efd1d-497">Полный метод `StreamingController.UploadDatabase` для потоковой передачи в базу данных с EF Core:</span><span class="sxs-lookup"><span data-stu-id="efd1d-497">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="efd1d-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="efd1d-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="efd1d-499">Полный метод `StreamingController.UploadPhysical` для потоковой передачи в физическое расположение:</span><span class="sxs-lookup"><span data-stu-id="efd1d-499">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="efd1d-500">В примере приложения проверки обрабатываются с помощью `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-500">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="efd1d-501">Проверка</span><span class="sxs-lookup"><span data-stu-id="efd1d-501">Validation</span></span>

<span data-ttu-id="efd1d-502">В классе `FileHelpers` примера приложения демонстрируется несколько проверок буферизованного файла <xref:Microsoft.AspNetCore.Http.IFormFile> и потоковой передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-502">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="efd1d-503">Сведения об обработке буферизованных файлов <xref:Microsoft.AspNetCore.Http.IFormFile> в примере приложения см. в описании метода `ProcessFormFile` в файле *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="efd1d-503">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="efd1d-504">Сведения об обработке потоковых файлов см. в описании метода `ProcessStreamedFile` в том же файле.</span><span class="sxs-lookup"><span data-stu-id="efd1d-504">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="efd1d-505">Методы обработки проверки, показанные в примере приложения, не проверяют содержимое отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-505">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="efd1d-506">В большинстве рабочих сценариев в файле применяется API сканирования на наличие вирусов и вредоносных программ, прежде чем сделать файл доступным для пользователей или других систем.</span><span class="sxs-lookup"><span data-stu-id="efd1d-506">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="efd1d-507">Хотя пример в разделе содержит рабочий пример методов проверки, не следует реализовывать класс `FileHelpers` в рабочем приложении, кроме таких случаев:</span><span class="sxs-lookup"><span data-stu-id="efd1d-507">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="efd1d-508">Вы полностью разбираетесь в реализации.</span><span class="sxs-lookup"><span data-stu-id="efd1d-508">Fully understand the implementation.</span></span>
> * <span data-ttu-id="efd1d-509">Вы изменяете реализацию соответствующим образом для среды и спецификаций приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-509">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="efd1d-510">**Никогда не реализуйте код безопасности в приложении, не выполнив эти требования.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-510">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="efd1d-511">Проверка содержимого</span><span class="sxs-lookup"><span data-stu-id="efd1d-511">Content validation</span></span>

<span data-ttu-id="efd1d-512">**Используйте сторонний API сканирования на наличие вирусов и вредоносных программ для отправленного содержимого.**</span><span class="sxs-lookup"><span data-stu-id="efd1d-512">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="efd1d-513">Сканирование файлов требует использования ресурсов сервера в сценариях с большими объемами данных.</span><span class="sxs-lookup"><span data-stu-id="efd1d-513">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="efd1d-514">Если производительность обработки запросов снижается из-за сканирования файлов, рассмотрите возможность разгрузки сканирования путем использования [фоновой службы](xref:fundamentals/host/hosted-services), возможно, службы, которая работает на сервере, отличном от сервера приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-514">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="efd1d-515">Как правило, передаваемые файлы хранятся в карантинной области до тех пор, пока фоновый сканер не проверит их на наличие вирусов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-515">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="efd1d-516">При передаче файл перемещается в нормальное место хранения файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-516">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="efd1d-517">Эти действия обычно выполняются вместе с записью базы данных, которая указывает состояние сканирования файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-517">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="efd1d-518">Благодаря такому подходу приложение и сервер приложений остаются в режиме реагирования на запросы.</span><span class="sxs-lookup"><span data-stu-id="efd1d-518">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="efd1d-519">Проверка расширения файла</span><span class="sxs-lookup"><span data-stu-id="efd1d-519">File extension validation</span></span>

<span data-ttu-id="efd1d-520">Расширение переданного файла должно проверяться в соответствии со списком разрешенных расширений.</span><span class="sxs-lookup"><span data-stu-id="efd1d-520">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="efd1d-521">Например:</span><span class="sxs-lookup"><span data-stu-id="efd1d-521">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="efd1d-522">Проверка подписи файла</span><span class="sxs-lookup"><span data-stu-id="efd1d-522">File signature validation</span></span>

<span data-ttu-id="efd1d-523">Подпись файла определяется по первым нескольким байтам в начале файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-523">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="efd1d-524">Эти байты можно использовать, чтобы указать, совпадает ли расширение с содержимым файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-524">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="efd1d-525">Пример приложения проверяет подписи файлов на соответствие нескольким распространенным типам файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-525">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="efd1d-526">В следующем примере проверяется подпись файла для изображения JPEG:</span><span class="sxs-lookup"><span data-stu-id="efd1d-526">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="efd1d-527">Дополнительные сведения о получении дополнительных подписей файлов см. [по этой ссылке](https://www.filesignatures.net/) и в официальных спецификациях файла.</span><span class="sxs-lookup"><span data-stu-id="efd1d-527">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="efd1d-528">Безопасность имени файла</span><span class="sxs-lookup"><span data-stu-id="efd1d-528">File name security</span></span>

<span data-ttu-id="efd1d-529">Никогда не используйте для хранения файла в физическом хранилище имя, предоставляемое клиентом.</span><span class="sxs-lookup"><span data-stu-id="efd1d-529">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="efd1d-530">Создайте надежное имя файла с помощью [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) или [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы получить полный путь (включая имя файла) для временного хранилища.</span><span class="sxs-lookup"><span data-stu-id="efd1d-530">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="efd1d-531">Razor автоматически кодирует в HTML значения свойств для вывода.</span><span class="sxs-lookup"><span data-stu-id="efd1d-531">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="efd1d-532">Ниже приведен безопасный код, который можно использовать.</span><span class="sxs-lookup"><span data-stu-id="efd1d-532">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="efd1d-533">Вне Razor содержимое имени файла из запроса пользователя всегда кодируется в <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="efd1d-533">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="efd1d-534">Во многих реализациях следует включать проверку существования файла. В противном случае файл перезаписывается файлом с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="efd1d-534">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="efd1d-535">Предоставьте дополнительную логику для соответствия спецификациям приложения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-535">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="efd1d-536">Проверка размера</span><span class="sxs-lookup"><span data-stu-id="efd1d-536">Size validation</span></span>

<span data-ttu-id="efd1d-537">Ограничьте размер передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="efd1d-537">Limit the size of uploaded files.</span></span>

<span data-ttu-id="efd1d-538">В примере приложения размер файла ограничен 2 МБ (указывается в байтах).</span><span class="sxs-lookup"><span data-stu-id="efd1d-538">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="efd1d-539">Ограничение предоставляется с помощью [конфигурации](xref:fundamentals/configuration/index) из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="efd1d-539">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="efd1d-540">В классы `PageModel` внедряется `FileSizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-540">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="efd1d-541">Если размер файла превышает ограничение, файл отклоняется:</span><span class="sxs-lookup"><span data-stu-id="efd1d-541">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="efd1d-542">Сопоставление значения атрибута имени и имени параметра метода POST</span><span class="sxs-lookup"><span data-stu-id="efd1d-542">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="efd1d-543">В формах, не относящихся к Razor, которые передают данные формы или используют `FormData` JavaScript напрямую, имя, указанное в элементе формы или `FormData`, должно соответствовать имени параметра в действии контроллера.</span><span class="sxs-lookup"><span data-stu-id="efd1d-543">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="efd1d-544">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="efd1d-544">In the following example:</span></span>

* <span data-ttu-id="efd1d-545">При использовании элемента `<input>` атрибуту `name` присваивается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-545">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="efd1d-546">При использовании `FormData` в JavaScript для имени задается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-546">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="efd1d-547">Используйте соответствующее имя для параметра метода C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="efd1d-547">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="efd1d-548">Для метода обработчика страницы Razor Pages с именем `Upload`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-548">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="efd1d-549">Для метода действия контроллера POST приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="efd1d-549">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="efd1d-550">Конфигурация сервера и приложения</span><span class="sxs-lookup"><span data-stu-id="efd1d-550">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="efd1d-551">Ограничение длины составного текста</span><span class="sxs-lookup"><span data-stu-id="efd1d-551">Multipart body length limit</span></span>

<span data-ttu-id="efd1d-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> устанавливает ограничение длины каждого составного текста.</span><span class="sxs-lookup"><span data-stu-id="efd1d-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="efd1d-553">Разделы формы, превышающие это ограничение, вызовут <xref:System.IO.InvalidDataException> при синтаксическом анализе.</span><span class="sxs-lookup"><span data-stu-id="efd1d-553">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="efd1d-554">Значение по умолчанию — 134 217 728 байт (128 МБ).</span><span class="sxs-lookup"><span data-stu-id="efd1d-554">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="efd1d-555">Настройте ограничение с помощью параметра <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-555">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="efd1d-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> используется для настройки <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="efd1d-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="efd1d-557">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-557">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="efd1d-558">В приложении Razor Pages или MVC примените фильтр к модели страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="efd1d-558">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="efd1d-559">Максимальный размер текста запроса Kestrel</span><span class="sxs-lookup"><span data-stu-id="efd1d-559">Kestrel maximum request body size</span></span>

<span data-ttu-id="efd1d-560">Для приложений, размещенных в Kestrel, по умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-560">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="efd1d-561">Настройте ограничение с помощью параметра сервера Kestrel [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size):</span><span class="sxs-lookup"><span data-stu-id="efd1d-561">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<span data-ttu-id="efd1d-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> используется для настройки [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="efd1d-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="efd1d-563">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd1d-563">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="efd1d-564">В приложении Razor Pages или MVC примените фильтр к классу обработчика страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="efd1d-564">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="efd1d-565">Другие ограничения Kestrel</span><span class="sxs-lookup"><span data-stu-id="efd1d-565">Other Kestrel limits</span></span>

<span data-ttu-id="efd1d-566">Другие ограничения Kestrel могут применяться для приложений, размещенных в Kestrel:</span><span class="sxs-lookup"><span data-stu-id="efd1d-566">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="efd1d-567">Максимальное число клиентских подключений</span><span class="sxs-lookup"><span data-stu-id="efd1d-567">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="efd1d-568">Скорость передачи данных запросов и ответов</span><span class="sxs-lookup"><span data-stu-id="efd1d-568">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="efd1d-569">Ограничение длины содержимого IIS</span><span class="sxs-lookup"><span data-stu-id="efd1d-569">IIS content length limit</span></span>

<span data-ttu-id="efd1d-570">По умолчанию ограничение запроса (`maxAllowedContentLength`) составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-570">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="efd1d-571">Ограничение можно настроить в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="efd1d-571">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="efd1d-572">Этот параметр применим только к службам IIS.</span><span class="sxs-lookup"><span data-stu-id="efd1d-572">This setting only applies to IIS.</span></span> <span data-ttu-id="efd1d-573">При размещении в Kestrel такой проблемы возникать не должно.</span><span class="sxs-lookup"><span data-stu-id="efd1d-573">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="efd1d-574">Дополнительные сведения см. в статье [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Ограничения запроса <requestLimits>).</span><span class="sxs-lookup"><span data-stu-id="efd1d-574">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="efd1d-575">Ограничения в модуле ASP.NET Core или наличие модуля фильтрации запросов IIS могут ограничить передачу до 2 или 4 ГБ.</span><span class="sxs-lookup"><span data-stu-id="efd1d-575">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="efd1d-576">Дополнительные сведения см. в [этой ветке форума](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="efd1d-576">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="efd1d-577">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="efd1d-577">Troubleshoot</span></span>

<span data-ttu-id="efd1d-578">Ниже описываются некоторые распространенные проблемы, которые возникают при передаче файлов, и возможные способы их решения.</span><span class="sxs-lookup"><span data-stu-id="efd1d-578">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="efd1d-579">Ошибка "Не найдено" при развертывании на сервере IIS</span><span class="sxs-lookup"><span data-stu-id="efd1d-579">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="efd1d-580">Следующая ошибка свидетельствует о том, что размер передаваемых файлов превышает настроенное на сервере ограничение длины содержимого:</span><span class="sxs-lookup"><span data-stu-id="efd1d-580">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="efd1d-581">Дополнительные сведения об увеличении предела см. в разделе [Ограничение длины содержимого IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="efd1d-581">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="efd1d-582">Сбой подключения</span><span class="sxs-lookup"><span data-stu-id="efd1d-582">Connection failure</span></span>

<span data-ttu-id="efd1d-583">Ошибка соединения и сброс соединения с сервером, скорее всего, означают, что размер отправленного файла превышает максимальную длину текста запроса Kestrel.</span><span class="sxs-lookup"><span data-stu-id="efd1d-583">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="efd1d-584">Дополнительные сведения см. в разделе [Максимальный размер текста запроса Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="efd1d-584">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="efd1d-585">Может также потребоваться настроить ограничения подключений клиентов Kestrel.</span><span class="sxs-lookup"><span data-stu-id="efd1d-585">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="efd1d-586">Исключение, связанное с пустой ссылкой в IFormFile</span><span class="sxs-lookup"><span data-stu-id="efd1d-586">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="efd1d-587">Если контроллер принимает передаваемые файлы с помощью <xref:Microsoft.AspNetCore.Http.IFormFile>, но значение равно `null`, проверьте, указан ли в HTML-форме атрибут `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-587">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="efd1d-588">Если этот атрибут не задан для элемента `<form>`, передача файлов происходить не будет и все связанные аргументы <xref:Microsoft.AspNetCore.Http.IFormFile> будут иметь значение `null`.</span><span class="sxs-lookup"><span data-stu-id="efd1d-588">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="efd1d-589">Убедитесь также, что [именование передачи в данных формы совпадает с именованием приложения](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="efd1d-589">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="efd1d-590">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="efd1d-590">Additional resources</span></span>

* [<span data-ttu-id="efd1d-591">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="efd1d-591">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="efd1d-592">Безопасность в Azure. Механизм безопасности. Проверки входных данных | Снижение риска</span><span class="sxs-lookup"><span data-stu-id="efd1d-592">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="efd1d-593">Конструктивные шаблоны облачных решений Azure. Шаблон ключа камердинера</span><span class="sxs-lookup"><span data-stu-id="efd1d-593">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
