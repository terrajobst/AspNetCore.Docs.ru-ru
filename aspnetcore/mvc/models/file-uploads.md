---
title: Передача файлов в ASP.NET Core
author: guardrex
description: Сведения об использовании привязки модели и потоковой передачи для передачи файлов в ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: de8bfee22e39dfc5a6ed254cf0555887891d4590
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179310"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="0cb35-103">Передача файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0cb35-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="0cb35-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham), [Стив Смит](https://ardalis.com/) (Steve Smith) и [Рутгер Сторм](https://github.com/rutix) (Rutger Storm)</span><span class="sxs-lookup"><span data-stu-id="0cb35-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0cb35-105">Действия ASP.NET Core поддерживают передачу одного или нескольких файлов с помощью привязки модели с буферизацией для небольших файлов или потоковой передачи без буферизации для более крупных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="0cb35-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0cb35-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0cb35-107">Замечания по безопасности</span><span class="sxs-lookup"><span data-stu-id="0cb35-107">Security considerations</span></span>

<span data-ttu-id="0cb35-108">Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер.</span><span class="sxs-lookup"><span data-stu-id="0cb35-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0cb35-109">Злоумышленники могут попытаться:</span><span class="sxs-lookup"><span data-stu-id="0cb35-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="0cb35-110">Выполнить атаку типа [отказ в обслуживании](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="0cb35-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="0cb35-111">Передать вирусы и вредоносные программы.</span><span class="sxs-lookup"><span data-stu-id="0cb35-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="0cb35-112">Нарушить безопасность сетей и серверов другими способами.</span><span class="sxs-lookup"><span data-stu-id="0cb35-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="0cb35-113">Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак.</span><span class="sxs-lookup"><span data-stu-id="0cb35-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0cb35-114">Передавайте файлы в выделенную область для отправки файлов в системе, желательно не на системный диск.</span><span class="sxs-lookup"><span data-stu-id="0cb35-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="0cb35-115">Использование выделенного расположения упрощает применение мер безопасности к отправленным файлам.</span><span class="sxs-lookup"><span data-stu-id="0cb35-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="0cb35-116">Отключите разрешения на выполнение для расположения отправки файла.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="0cb35-117">Никогда не сохраняйте переданные файлы в дереве каталогов, где находится приложение.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="0cb35-118">Используйте безопасное имя файла, определяемое приложением.</span><span class="sxs-lookup"><span data-stu-id="0cb35-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="0cb35-119">Не используйте имя файла, предоставленное пользователем, или ненадежное имя переданного файла.&dagger; Для вывода ненадежного имени файла в пользовательском интерфейсе или в сообщении журнала закодируйте значение в HTML.</span><span class="sxs-lookup"><span data-stu-id="0cb35-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="0cb35-120">Разрешите только определенный набор одобренных расширений файлов.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="0cb35-121">Проверьте подпись формата файла, чтобы запретить пользователю отправку подменяющего файла.&dagger; Например, запретите пользователю отправлять *EXE*-файл с расширением *TXT*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="0cb35-122">Убедитесь, что на сервере выполняются проверки на стороне клиента.&dagger; Проверки на стороне клиента можно легко обойти.</span><span class="sxs-lookup"><span data-stu-id="0cb35-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0cb35-123">Проверяйте размер отправляемых файлов и запретите выполнять отправку файлов большего размера.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="0cb35-124">Если файлы не должны перезаписываться переданным файлом с тем же именем, перед отправкой файла проверьте его имя в базе данных или физическом хранилище.</span><span class="sxs-lookup"><span data-stu-id="0cb35-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="0cb35-125">**Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ, прежде чем сохранять файл.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="0cb35-126">&dagger;Пример приложения демонстрирует подход, который соответствует критериям.</span><span class="sxs-lookup"><span data-stu-id="0cb35-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-127">Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:</span><span class="sxs-lookup"><span data-stu-id="0cb35-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="0cb35-128">полностью получить контроль над системой;</span><span class="sxs-lookup"><span data-stu-id="0cb35-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="0cb35-129">перезагрузить систему так, что она окажется в неработоспособном состоянии;</span><span class="sxs-lookup"><span data-stu-id="0cb35-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="0cb35-130">скомпрометировать пользовательские или системные данные;</span><span class="sxs-lookup"><span data-stu-id="0cb35-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="0cb35-131">применить граффити к открытому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="0cb35-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="0cb35-132">Сведения об уменьшении контактной зоны атаки во время приема файлов от пользователей см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="0cb35-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="0cb35-133">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="0cb35-133">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="0cb35-134">[Безопасность в Azure. Убедитесь, что при приеме файлов от пользователей обеспечиваются соответствующие меры безопасности](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users).</span><span class="sxs-lookup"><span data-stu-id="0cb35-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="0cb35-135">Дополнительные сведения о реализации мер безопасности, включая примеры из примера приложения, см. в статье [Передача файлов в ASP.NET Core](#validation).</span><span class="sxs-lookup"><span data-stu-id="0cb35-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="0cb35-136">Сценарии использования хранилища</span><span class="sxs-lookup"><span data-stu-id="0cb35-136">Storage scenarios</span></span>

<span data-ttu-id="0cb35-137">К общим вариантам хранилища файлов относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="0cb35-137">Common storage options for files include:</span></span>

* <span data-ttu-id="0cb35-138">База данных</span><span class="sxs-lookup"><span data-stu-id="0cb35-138">Database</span></span>

  * <span data-ttu-id="0cb35-139">В случае отправки небольших файлов база данных часто работает быстрее, чем физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="0cb35-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="0cb35-140">База данных часто более удобна по сравнению с вариантами физического хранилища, так как получение записи из базы пользовательских данных может одновременно предоставить содержимое файла (например, изображение аватара).</span><span class="sxs-lookup"><span data-stu-id="0cb35-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="0cb35-141">Эксплуатация базы данных потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="0cb35-142">Физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="0cb35-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="0cb35-143">Обработка передачи больших файлов:</span><span class="sxs-lookup"><span data-stu-id="0cb35-143">For large file uploads:</span></span>
    * <span data-ttu-id="0cb35-144">Ограничения базы данных могут ограничивать размер передачи.</span><span class="sxs-lookup"><span data-stu-id="0cb35-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="0cb35-145">Физическое хранилище часто менее экономически выгодно, чем хранилище в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="0cb35-146">Эксплуатация физического хранилища потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="0cb35-147">Процесс приложения должен иметь разрешения на чтение и запись для места хранения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="0cb35-148">**Никогда не предоставляйте разрешение на выполнение.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="0cb35-149">Служба хранилища данных (например, хранилище [BLOB-объектов Azure](https://azure.microsoft.com/services/storage/blobs/)).</span><span class="sxs-lookup"><span data-stu-id="0cb35-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="0cb35-150">Обычно службы обеспечивают улучшенную масштабируемость и устойчивость по сравнению с локальными решениями, которые обычно подвержены единым точкам отказа.</span><span class="sxs-lookup"><span data-stu-id="0cb35-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="0cb35-151">Затраты на использование служб обычно ниже в сценариях с крупномасштабной инфраструктурой хранения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="0cb35-152">Дополнительные сведения см. в разделе [Краткое руководство. Использование .NET для создания большого двоичного объекта в хранилище объектов](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="0cb35-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="0cb35-153">В этом разделе показан метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, но метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> можно использовать для сохранения <xref:System.IO.FileStream> в хранилище BLOB-объектов при работе с <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="0cb35-154">Сценарии передачи файлов</span><span class="sxs-lookup"><span data-stu-id="0cb35-154">File upload scenarios</span></span>

<span data-ttu-id="0cb35-155">Есть два распространенных подхода к передаче файлов — буферизация и потоковая передача.</span><span class="sxs-lookup"><span data-stu-id="0cb35-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="0cb35-156">**Буферизация**</span><span class="sxs-lookup"><span data-stu-id="0cb35-156">**Buffering**</span></span>

<span data-ttu-id="0cb35-157">Весь файл считывается в <xref:Microsoft.AspNetCore.Http.IFormFile> (представление файла на C#, используемого для обработки или сохранения файла).</span><span class="sxs-lookup"><span data-stu-id="0cb35-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="0cb35-158">Потребление ресурсов (диска, памяти) при передаче файлов зависит от количества и размера одновременно передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="0cb35-159">При попытке приложения поместить в буфер слишком много файлов может произойти аварийное завершение работы сайта из-за нехватки памяти или места на диске.</span><span class="sxs-lookup"><span data-stu-id="0cb35-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="0cb35-160">Если размер или частота отправки файлов исчерпывают ресурсы приложения, используйте потоковую передачу.</span><span class="sxs-lookup"><span data-stu-id="0cb35-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="0cb35-161">Один буферизованный файл размером свыше 64 КБ перемещается из памяти во временный файл на диске.</span><span class="sxs-lookup"><span data-stu-id="0cb35-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="0cb35-162">Буферизация небольших файлов описана в следующих разделах этой статьи:</span><span class="sxs-lookup"><span data-stu-id="0cb35-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="0cb35-163">Физическое хранилище</span><span class="sxs-lookup"><span data-stu-id="0cb35-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="0cb35-164">База данных</span><span class="sxs-lookup"><span data-stu-id="0cb35-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="0cb35-165">**Потоковая передача**</span><span class="sxs-lookup"><span data-stu-id="0cb35-165">**Streaming**</span></span>

<span data-ttu-id="0cb35-166">Файл можно получить с помощью составного запроса. Затем он обрабатывается или сохраняется приложением напрямую.</span><span class="sxs-lookup"><span data-stu-id="0cb35-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="0cb35-167">Потоковая передача повышает производительность не значительно.</span><span class="sxs-lookup"><span data-stu-id="0cb35-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="0cb35-168">При отправке файлов потоковая передача снижает нагрузку на память или на место на диске.</span><span class="sxs-lookup"><span data-stu-id="0cb35-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="0cb35-169">Потоковая передача больших файлов рассматривается в разделе [Передача больших файлов с помощью потоковой передачи](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0cb35-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="0cb35-170">Передача небольших файлов с привязкой буферизованной модели к физическому хранилищу</span><span class="sxs-lookup"><span data-stu-id="0cb35-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="0cb35-171">Для передачи небольших файлов можно применить составную форму или сформировать запрос POST на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cb35-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="0cb35-172">В следующем примере демонстрируется использование формы Razor Pages для передачи одного файла (*Pages/BufferedSingleFileUploadPhysical.cshtml* в примере приложения):</span><span class="sxs-lookup"><span data-stu-id="0cb35-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="0cb35-173">Следующий пример аналогичен предыдущему примеру, за исключением следующего:</span><span class="sxs-lookup"><span data-stu-id="0cb35-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="0cb35-174">JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) используется для отправки данных формы.</span><span class="sxs-lookup"><span data-stu-id="0cb35-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="0cb35-175">Проверка не выполняется.</span><span class="sxs-lookup"><span data-stu-id="0cb35-175">There's no validation.</span></span>

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

<span data-ttu-id="0cb35-176">Для выполнения отправки формы в JavaScript для клиентов, которые [не поддерживают Fetch API](https://caniuse.com/#feat=fetch), используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="0cb35-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="0cb35-177">Используйте функцию Fetch Polyfill (например, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="0cb35-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="0cb35-178">Используйте ключевое слово `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="0cb35-179">Например:</span><span class="sxs-lookup"><span data-stu-id="0cb35-179">For example:</span></span>

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

<span data-ttu-id="0cb35-180">Для поддержки передачи файлов в HTML-формах должен указываться тип кодировки `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="0cb35-181">Для элемента ввода `files`, поддерживающего отправку нескольких файлов, в элементе `<input>` необходимо указать атрибут `multiple`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="0cb35-182">Доступ к отдельным файлам, переданным на сервер, можно получать посредством [привязки модели](xref:mvc/models/model-binding) с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="0cb35-183">В примере приложения показано несколько отправок буферизованных файлов для баз данных и физических хранилищ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-184">Свойство `FileName` параметра <xref:Microsoft.AspNetCore.Http.IFormFile> требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="0cb35-185">Свойство `FileName` следует использовать только в целях вывода и только после HTML-кодирования значения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="0cb35-186">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="0cb35-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="0cb35-187">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="0cb35-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="0cb35-188">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="0cb35-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="0cb35-189">Проверка</span><span class="sxs-lookup"><span data-stu-id="0cb35-189">Validation</span></span>](#validation)

<span data-ttu-id="0cb35-190">При отправке файлов с помощью привязки модели и <xref:Microsoft.AspNetCore.Http.IFormFile> метод действия может принимать следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="0cb35-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="0cb35-191">Один файл <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="0cb35-192">Любая из следующих коллекций, представляющих несколько файлов:</span><span class="sxs-lookup"><span data-stu-id="0cb35-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="0cb35-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="0cb35-194">Привязка сопоставляет файлы форм по имени.</span><span class="sxs-lookup"><span data-stu-id="0cb35-194">Binding matches form files by name.</span></span> <span data-ttu-id="0cb35-195">Например, значение HTML `name` в `<input type="file" name="formFile">` должно соответствовать привязанному к C# параметру или свойству (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="0cb35-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="0cb35-196">Дополнительные сведения см. в разделе [Сопоставление значения атрибута имени и имени параметра метода POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="0cb35-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="0cb35-197">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="0cb35-197">The following example:</span></span>

* <span data-ttu-id="0cb35-198">Циклично отправляет один или несколько передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="0cb35-199">Использует метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы вернуть полный путь к файлу, включая его имя.</span><span class="sxs-lookup"><span data-stu-id="0cb35-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="0cb35-200">Сохраняет файлы в локальную файловую систему, используя имя файла, созданное приложением.</span><span class="sxs-lookup"><span data-stu-id="0cb35-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="0cb35-201">Возвращает общее число и размер отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="0cb35-202">Чтобы создать имя файла без пути, используйте `Path.GetRandomFileName`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="0cb35-203">В следующем примере путь получен из конфигурации:</span><span class="sxs-lookup"><span data-stu-id="0cb35-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="0cb35-204">Передаваемый в <xref:System.IO.FileStream> путь *должен* содержать имя файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="0cb35-205">Если имя файла не указано, в среде выполнения возникает исключение <xref:System.UnauthorizedAccessException>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="0cb35-206">Файлы, передаваемые с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>, буферизуются в памяти или на диске на сервере перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="0cb35-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="0cb35-207">Внутри метода действия содержимое <xref:Microsoft.AspNetCore.Http.IFormFile> доступно в виде <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="0cb35-208">Помимо локальной файловой системы, файлы можно сохранять в сетевой папке или в службе хранилища файлов, например в хранилище [BLOB-объектов Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="0cb35-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="0cb35-209">Еще один пример, который перебирает несколько файлов для отправки и использует надежные имена файлов, см. в файле *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-210">Метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) вызывает исключение <xref:System.IO.IOException> в случае создания более чем 65 535 файлов без удаления предыдущих временных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="0cb35-211">Ограничение в 65 535 файлов предусмотрено для каждого сервера.</span><span class="sxs-lookup"><span data-stu-id="0cb35-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="0cb35-212">Дополнительные сведения об этом ограничении в ОС Windows см. в примечаниях в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="0cb35-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * <span data-ttu-id="0cb35-213">[Функция GetTempFileNameA](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks);</span><span class="sxs-lookup"><span data-stu-id="0cb35-213">[GetTempFileNameA function](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)</span></span>
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="0cb35-214">Передача небольших файлов с привязкой буферизованной модели к базе данных</span><span class="sxs-lookup"><span data-stu-id="0cb35-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="0cb35-215">Для сохранения данных двоичных файлов в базе данных с помощью [Entity Framework](/ef/core/index) определите для сущности свойство массива <xref:System.Byte>:</span><span class="sxs-lookup"><span data-stu-id="0cb35-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="0cb35-216">Укажите свойство модели страницы для класса, который содержит <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="0cb35-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="0cb35-217"><xref:Microsoft.AspNetCore.Http.IFormFile> можно использовать непосредственно как параметр метода действия или свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="0cb35-218">В предыдущем примере используется свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="0cb35-219">В форме Razor Pages используется `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="0cb35-220">При публикации формы на сервере скопируйте <xref:Microsoft.AspNetCore.Http.IFormFile> в поток и сохраните его в базе данных в виде массива байтов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="0cb35-221">В следующем примере `_dbContext` сохраняет контекст базы данных приложения:</span><span class="sxs-lookup"><span data-stu-id="0cb35-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="0cb35-222">Предыдущий пример похож на сценарий, продемонстрированный в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="0cb35-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="0cb35-223">*Pages/BufferedSingleFileUploadDb.cshtml*;</span><span class="sxs-lookup"><span data-stu-id="0cb35-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="0cb35-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-225">При сохранении двоичных данных в реляционных базах данных следует соблюдать осторожность, так как это может отрицательно сказаться на производительности.</span><span class="sxs-lookup"><span data-stu-id="0cb35-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="0cb35-226">Свойство `FileName` параметра <xref:Microsoft.AspNetCore.Http.IFormFile> требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="0cb35-227">Свойство `FileName` следует использовать только в целях вывода и только после HTML-кодирования.</span><span class="sxs-lookup"><span data-stu-id="0cb35-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="0cb35-228">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="0cb35-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="0cb35-229">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="0cb35-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="0cb35-230">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="0cb35-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="0cb35-231">Проверка</span><span class="sxs-lookup"><span data-stu-id="0cb35-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="0cb35-232">Передача больших файлов с помощью потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="0cb35-232">Upload large files with streaming</span></span>

<span data-ttu-id="0cb35-233">В приведенном ниже примере демонстрируется использование JavaScript для потоковой передачи файла в действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="0cb35-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="0cb35-234">Токен против подделки файла создается с помощью пользовательского атрибута фильтра и передается в заголовках HTTP клиента, а не в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="0cb35-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="0cb35-235">Так как метод действия обрабатывает передаваемые данные напрямую, привязка модели формы отключается другим пользовательским фильтром.</span><span class="sxs-lookup"><span data-stu-id="0cb35-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="0cb35-236">Внутри действия содержимое формы считывается с помощью объекта `MultipartReader`, который считывает каждый объект `MultipartSection` по отдельности, обрабатывая файл или сохраняя содержимое.</span><span class="sxs-lookup"><span data-stu-id="0cb35-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="0cb35-237">После считывания составных разделов действие выполняет собственную привязку модели.</span><span class="sxs-lookup"><span data-stu-id="0cb35-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="0cb35-238">Вначале страница загружает форму и сохраняет токен против подделки в файле cookie (с помощью атрибута `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="0cb35-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="0cb35-239">Атрибут использует встроенную поддержку [защиты от подделки](xref:security/anti-request-forgery) в ASP.NET Core, чтобы задать файл cookie с токеном запроса.</span><span class="sxs-lookup"><span data-stu-id="0cb35-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="0cb35-240">Для отключения привязки модели используется `DisableFormValueModelBindingAttribute`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="0cb35-241">В примере приложения `GenerateAntiforgeryTokenCookieAttribute` и `DisableFormValueModelBindingAttribute` применяются в качестве фильтров к моделям приложений на странице `/StreamedSingleFileUploadDb` и `/StreamedSingleFileUploadPhysical` в `Startup.ConfigureServices` с использованием [соглашений Razor Pages](xref:razor-pages/razor-pages-conventions).</span><span class="sxs-lookup"><span data-stu-id="0cb35-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="0cb35-242">Так как привязка модели не считывает форму, параметры, привязанные из формы, не привязываются (запросы, маршруты и заголовки продолжают работать).</span><span class="sxs-lookup"><span data-stu-id="0cb35-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="0cb35-243">Метод действия работает напрямую со свойством `Request`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="0cb35-244">Для считывания каждого раздела служит объект `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="0cb35-245">Данные "ключ — значение" хранятся в `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="0cb35-246">После считывания составных разделов содержимое `KeyValueAccumulator` используется для привязки данных формы к типу модели.</span><span class="sxs-lookup"><span data-stu-id="0cb35-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="0cb35-247">Полный метод `StreamingController.UploadDatabase` для потоковой передачи в базу данных с EF Core:</span><span class="sxs-lookup"><span data-stu-id="0cb35-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="0cb35-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="0cb35-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="0cb35-249">Полный метод `StreamingController.UploadPhysical` для потоковой передачи в физическое расположение:</span><span class="sxs-lookup"><span data-stu-id="0cb35-249">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="0cb35-250">В примере приложения проверки обрабатываются с помощью `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-250">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="0cb35-251">Проверка</span><span class="sxs-lookup"><span data-stu-id="0cb35-251">Validation</span></span>

<span data-ttu-id="0cb35-252">В классе `FileHelpers` примера приложения демонстрируется несколько проверок буферизованного файла <xref:Microsoft.AspNetCore.Http.IFormFile> и потоковой передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-252">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="0cb35-253">Сведения об обработке буферизованных файлов <xref:Microsoft.AspNetCore.Http.IFormFile> в примере приложения см. в описании метода `ProcessFormFile` в файле *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-253">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="0cb35-254">Сведения об обработке потоковых файлов см. в описании метода `ProcessStreamedFile` в том же файле.</span><span class="sxs-lookup"><span data-stu-id="0cb35-254">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-255">Методы обработки проверки, показанные в примере приложения, не проверяют содержимое отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-255">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="0cb35-256">В большинстве рабочих сценариев в файле применяется API сканирования на наличие вирусов и вредоносных программ, прежде чем сделать файл доступным для пользователей или других систем.</span><span class="sxs-lookup"><span data-stu-id="0cb35-256">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="0cb35-257">Хотя пример в разделе содержит рабочий пример методов проверки, не следует реализовывать класс `FileHelpers` в рабочем приложении, кроме таких случаев:</span><span class="sxs-lookup"><span data-stu-id="0cb35-257">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="0cb35-258">Вы полностью разбираетесь в реализации.</span><span class="sxs-lookup"><span data-stu-id="0cb35-258">Fully understand the implementation.</span></span>
> * <span data-ttu-id="0cb35-259">Вы изменяете реализацию соответствующим образом для среды и спецификаций приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-259">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="0cb35-260">**Никогда не реализуйте код безопасности в приложении, не выполнив эти требования.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-260">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="0cb35-261">Проверка содержимого</span><span class="sxs-lookup"><span data-stu-id="0cb35-261">Content validation</span></span>

<span data-ttu-id="0cb35-262">**Используйте сторонний API сканирования на наличие вирусов и вредоносных программ для отправленного содержимого.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-262">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="0cb35-263">Сканирование файлов требует использования ресурсов сервера в сценариях с большими объемами данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-263">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="0cb35-264">Если производительность обработки запросов снижается из-за сканирования файлов, рассмотрите возможность разгрузки сканирования путем использования [фоновой службы](xref:fundamentals/host/hosted-services), возможно, службы, которая работает на сервере, отличном от сервера приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-264">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="0cb35-265">Как правило, передаваемые файлы хранятся в карантинной области до тех пор, пока фоновый сканер не проверит их на наличие вирусов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-265">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="0cb35-266">При передаче файл перемещается в нормальное место хранения файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-266">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="0cb35-267">Эти действия обычно выполняются вместе с записью базы данных, которая указывает состояние сканирования файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-267">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="0cb35-268">Благодаря такому подходу приложение и сервер приложений остаются в режиме реагирования на запросы.</span><span class="sxs-lookup"><span data-stu-id="0cb35-268">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="0cb35-269">Проверка расширения файла</span><span class="sxs-lookup"><span data-stu-id="0cb35-269">File extension validation</span></span>

<span data-ttu-id="0cb35-270">Расширение переданного файла должно проверяться в соответствии со списком разрешенных расширений.</span><span class="sxs-lookup"><span data-stu-id="0cb35-270">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="0cb35-271">Например:</span><span class="sxs-lookup"><span data-stu-id="0cb35-271">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="0cb35-272">Проверка подписи файла</span><span class="sxs-lookup"><span data-stu-id="0cb35-272">File signature validation</span></span>

<span data-ttu-id="0cb35-273">Подпись файла определяется по первым нескольким байтам в начале файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-273">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="0cb35-274">Эти байты можно использовать, чтобы указать, совпадает ли расширение с содержимым файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-274">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="0cb35-275">Пример приложения проверяет подписи файлов на соответствие нескольким распространенным типам файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-275">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="0cb35-276">В следующем примере проверяется подпись файла для изображения JPEG:</span><span class="sxs-lookup"><span data-stu-id="0cb35-276">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="0cb35-277">Дополнительные сведения о получении дополнительных подписей файлов см. [по этой ссылке](https://www.filesignatures.net/) и в официальных спецификациях файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-277">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="0cb35-278">Безопасность имени файла</span><span class="sxs-lookup"><span data-stu-id="0cb35-278">File name security</span></span>

<span data-ttu-id="0cb35-279">Никогда не используйте для хранения файла в физическом хранилище имя, предоставляемое клиентом.</span><span class="sxs-lookup"><span data-stu-id="0cb35-279">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="0cb35-280">Создайте надежное имя файла с помощью [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) или [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы получить полный путь (включая имя файла) для временного хранилища.</span><span class="sxs-lookup"><span data-stu-id="0cb35-280">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="0cb35-281">Razor автоматически кодирует в HTML значения свойств для вывода.</span><span class="sxs-lookup"><span data-stu-id="0cb35-281">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="0cb35-282">Ниже приведен безопасный код, который можно использовать.</span><span class="sxs-lookup"><span data-stu-id="0cb35-282">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="0cb35-283">Вне Razor содержимое имени файла из запроса пользователя всегда кодируется в <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-283">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="0cb35-284">Во многих реализациях следует включать проверку существования файла. В противном случае файл перезаписывается файлом с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="0cb35-284">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="0cb35-285">Предоставьте дополнительную логику для соответствия спецификациям приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-285">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="0cb35-286">Проверка размера</span><span class="sxs-lookup"><span data-stu-id="0cb35-286">Size validation</span></span>

<span data-ttu-id="0cb35-287">Ограничьте размер передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-287">Limit the size of uploaded files.</span></span>

<span data-ttu-id="0cb35-288">В примере приложения размер файла ограничен 2 МБ (указывается в байтах).</span><span class="sxs-lookup"><span data-stu-id="0cb35-288">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="0cb35-289">Ограничение предоставляется с помощью [конфигурации](xref:fundamentals/configuration/index) из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-289">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="0cb35-290">В классы `PageModel` внедряется `FileSizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-290">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="0cb35-291">Если размер файла превышает ограничение, файл отклоняется:</span><span class="sxs-lookup"><span data-stu-id="0cb35-291">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="0cb35-292">Сопоставление значения атрибута имени и имени параметра метода POST</span><span class="sxs-lookup"><span data-stu-id="0cb35-292">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="0cb35-293">В формах, не относящихся к Razor, которые передают данные формы или используют `FormData` JavaScript напрямую, имя, указанное в элементе формы или `FormData`, должно соответствовать имени параметра в действии контроллера.</span><span class="sxs-lookup"><span data-stu-id="0cb35-293">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="0cb35-294">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0cb35-294">In the following example:</span></span>

* <span data-ttu-id="0cb35-295">При использовании элемента `<input>` атрибуту `name` присваивается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-295">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="0cb35-296">При использовании `FormData` в JavaScript для имени задается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-296">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="0cb35-297">Используйте соответствующее имя для параметра метода C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="0cb35-297">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="0cb35-298">Для метода обработчика страницы Razor Pages с именем `Upload`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-298">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="0cb35-299">Для метода действия контроллера POST приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="0cb35-299">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="0cb35-300">Конфигурация сервера и приложения</span><span class="sxs-lookup"><span data-stu-id="0cb35-300">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="0cb35-301">Ограничение длины составного текста</span><span class="sxs-lookup"><span data-stu-id="0cb35-301">Multipart body length limit</span></span>

<span data-ttu-id="0cb35-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> устанавливает ограничение длины каждого составного текста.</span><span class="sxs-lookup"><span data-stu-id="0cb35-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="0cb35-303">Разделы формы, превышающие это ограничение, вызовут <xref:System.IO.InvalidDataException> при синтаксическом анализе.</span><span class="sxs-lookup"><span data-stu-id="0cb35-303">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="0cb35-304">Значение по умолчанию — 134 217 728 байт (128 МБ).</span><span class="sxs-lookup"><span data-stu-id="0cb35-304">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="0cb35-305">Настройте ограничение с помощью параметра <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-305">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="0cb35-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> используется для настройки <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="0cb35-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="0cb35-307">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-307">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="0cb35-308">В приложении Razor Pages или MVC примените фильтр к модели страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="0cb35-308">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="0cb35-309">Максимальный размер текста запроса Kestrel</span><span class="sxs-lookup"><span data-stu-id="0cb35-309">Kestrel maximum request body size</span></span>

<span data-ttu-id="0cb35-310">Для приложений, размещенных в Kestrel, по умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-310">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="0cb35-311">Настройте ограничение с помощью параметра сервера Kestrel [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size):</span><span class="sxs-lookup"><span data-stu-id="0cb35-311">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="0cb35-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> используется для настройки [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="0cb35-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="0cb35-313">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="0cb35-314">В приложении Razor Pages или MVC примените фильтр к классу обработчика страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="0cb35-314">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="0cb35-315">`RequestSizeLimitAttribute` можно также применить с помощью директивы Razor [@attribute](xref:mvc/views/razor#attribute).</span><span class="sxs-lookup"><span data-stu-id="0cb35-315">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="0cb35-316">Другие ограничения Kestrel</span><span class="sxs-lookup"><span data-stu-id="0cb35-316">Other Kestrel limits</span></span>

<span data-ttu-id="0cb35-317">Другие ограничения Kestrel могут применяться для приложений, размещенных в Kestrel:</span><span class="sxs-lookup"><span data-stu-id="0cb35-317">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="0cb35-318">Максимальное число клиентских подключений</span><span class="sxs-lookup"><span data-stu-id="0cb35-318">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="0cb35-319">Скорость передачи данных запросов и ответов</span><span class="sxs-lookup"><span data-stu-id="0cb35-319">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="0cb35-320">Ограничение длины содержимого IIS</span><span class="sxs-lookup"><span data-stu-id="0cb35-320">IIS content length limit</span></span>

<span data-ttu-id="0cb35-321">По умолчанию ограничение запроса (`maxAllowedContentLength`) составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-321">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="0cb35-322">Ограничение можно настроить в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="0cb35-322">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="0cb35-323">Этот параметр применим только к службам IIS.</span><span class="sxs-lookup"><span data-stu-id="0cb35-323">This setting only applies to IIS.</span></span> <span data-ttu-id="0cb35-324">При размещении в Kestrel такой проблемы возникать не должно.</span><span class="sxs-lookup"><span data-stu-id="0cb35-324">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="0cb35-325">Дополнительные сведения см. в статье [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Ограничения запроса <requestLimits>).</span><span class="sxs-lookup"><span data-stu-id="0cb35-325">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="0cb35-326">Ограничения в модуле ASP.NET Core или наличие модуля фильтрации запросов IIS могут ограничить передачу до 2 или 4 ГБ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-326">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="0cb35-327">Дополнительные сведения см. в [этой ветке форума](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="0cb35-327">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="0cb35-328">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="0cb35-328">Troubleshoot</span></span>

<span data-ttu-id="0cb35-329">Ниже описываются некоторые распространенные проблемы, которые возникают при передаче файлов, и возможные способы их решения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-329">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="0cb35-330">Ошибка "Не найдено" при развертывании на сервере IIS</span><span class="sxs-lookup"><span data-stu-id="0cb35-330">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="0cb35-331">Следующая ошибка свидетельствует о том, что размер передаваемых файлов превышает настроенное на сервере ограничение длины содержимого:</span><span class="sxs-lookup"><span data-stu-id="0cb35-331">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="0cb35-332">Дополнительные сведения об увеличении предела см. в разделе [Ограничение длины содержимого IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="0cb35-332">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="0cb35-333">Сбой подключения</span><span class="sxs-lookup"><span data-stu-id="0cb35-333">Connection failure</span></span>

<span data-ttu-id="0cb35-334">Ошибка соединения и сброс соединения с сервером, скорее всего, означают, что размер отправленного файла превышает максимальную длину текста запроса Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0cb35-334">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="0cb35-335">Дополнительные сведения см. в разделе [Максимальный размер текста запроса Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="0cb35-335">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="0cb35-336">Может также потребоваться настроить ограничения подключений клиентов Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0cb35-336">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="0cb35-337">Исключение, связанное с пустой ссылкой в IFormFile</span><span class="sxs-lookup"><span data-stu-id="0cb35-337">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="0cb35-338">Если контроллер принимает передаваемые файлы с помощью <xref:Microsoft.AspNetCore.Http.IFormFile>, но значение равно `null`, проверьте, указан ли в HTML-форме атрибут `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-338">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="0cb35-339">Если этот атрибут не задан для элемента `<form>`, передача файлов происходить не будет и все связанные аргументы <xref:Microsoft.AspNetCore.Http.IFormFile> будут иметь значение `null`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-339">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="0cb35-340">Убедитесь также, что [именование передачи в данных формы совпадает с именованием приложения](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="0cb35-340">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0cb35-341">Действия ASP.NET Core поддерживают передачу одного или нескольких файлов с помощью привязки модели с буферизацией для небольших файлов или потоковой передачи без буферизации для более крупных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-341">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="0cb35-342">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0cb35-342">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0cb35-343">Замечания по безопасности</span><span class="sxs-lookup"><span data-stu-id="0cb35-343">Security considerations</span></span>

<span data-ttu-id="0cb35-344">Необходимо соблюдать осторожность при предоставлении пользователям возможности отправки файлов на сервер.</span><span class="sxs-lookup"><span data-stu-id="0cb35-344">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0cb35-345">Злоумышленники могут попытаться:</span><span class="sxs-lookup"><span data-stu-id="0cb35-345">Attackers may attempt to:</span></span>

* <span data-ttu-id="0cb35-346">Выполнить атаку типа [отказ в обслуживании](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="0cb35-346">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="0cb35-347">Передать вирусы и вредоносные программы.</span><span class="sxs-lookup"><span data-stu-id="0cb35-347">Upload viruses or malware.</span></span>
* <span data-ttu-id="0cb35-348">Нарушить безопасность сетей и серверов другими способами.</span><span class="sxs-lookup"><span data-stu-id="0cb35-348">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="0cb35-349">Ниже приведены некоторые действия по обеспечению безопасности, которые снижают вероятность успешных атак.</span><span class="sxs-lookup"><span data-stu-id="0cb35-349">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0cb35-350">Передавайте файлы в выделенную область для отправки файлов в системе, желательно не на системный диск.</span><span class="sxs-lookup"><span data-stu-id="0cb35-350">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="0cb35-351">Использование выделенного расположения упрощает применение мер безопасности к отправленным файлам.</span><span class="sxs-lookup"><span data-stu-id="0cb35-351">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="0cb35-352">Отключите разрешения на выполнение для расположения отправки файла.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-352">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="0cb35-353">Никогда не сохраняйте переданные файлы в дереве каталогов, где находится приложение.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-353">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="0cb35-354">Используйте безопасное имя файла, определяемое приложением.</span><span class="sxs-lookup"><span data-stu-id="0cb35-354">Use a safe file name determined by the app.</span></span> <span data-ttu-id="0cb35-355">Не используйте имя файла, предоставленное пользователем, или ненадежное имя переданного файла.&dagger; Для вывода ненадежного имени файла в пользовательском интерфейсе или в сообщении журнала закодируйте значение в HTML.</span><span class="sxs-lookup"><span data-stu-id="0cb35-355">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="0cb35-356">Разрешите только определенный набор одобренных расширений файлов.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-356">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="0cb35-357">Проверьте подпись формата файла, чтобы запретить пользователю отправку подменяющего файла.&dagger; Например, запретите пользователю отправлять *EXE*-файл с расширением *TXT*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-357">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="0cb35-358">Убедитесь, что на сервере выполняются проверки на стороне клиента.&dagger; Проверки на стороне клиента можно легко обойти.</span><span class="sxs-lookup"><span data-stu-id="0cb35-358">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0cb35-359">Проверяйте размер отправляемых файлов и запретите выполнять отправку файлов большего размера.&dagger;</span><span class="sxs-lookup"><span data-stu-id="0cb35-359">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="0cb35-360">Если файлы не должны перезаписываться переданным файлом с тем же именем, перед отправкой файла проверьте его имя в базе данных или физическом хранилище.</span><span class="sxs-lookup"><span data-stu-id="0cb35-360">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="0cb35-361">**Запустите сканер для проверки отправляемого содержимого на наличие вирусов и вредоносных программ, прежде чем сохранять файл.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-361">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="0cb35-362">&dagger;Пример приложения демонстрирует подход, который соответствует критериям.</span><span class="sxs-lookup"><span data-stu-id="0cb35-362">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-363">Отправка в систему вредоносного кода часто является первым шагом перед выполнением кода, который может:</span><span class="sxs-lookup"><span data-stu-id="0cb35-363">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="0cb35-364">полностью получить контроль над системой;</span><span class="sxs-lookup"><span data-stu-id="0cb35-364">Completely gain control of a system.</span></span>
> * <span data-ttu-id="0cb35-365">перезагрузить систему так, что она окажется в неработоспособном состоянии;</span><span class="sxs-lookup"><span data-stu-id="0cb35-365">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="0cb35-366">скомпрометировать пользовательские или системные данные;</span><span class="sxs-lookup"><span data-stu-id="0cb35-366">Compromise user or system data.</span></span>
> * <span data-ttu-id="0cb35-367">применить граффити к открытому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="0cb35-367">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="0cb35-368">Сведения об уменьшении контактной зоны атаки во время приема файлов от пользователей см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="0cb35-368">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="0cb35-369">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="0cb35-369">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="0cb35-370">[Безопасность в Azure. Убедитесь, что при приеме файлов от пользователей обеспечиваются соответствующие меры безопасности](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users).</span><span class="sxs-lookup"><span data-stu-id="0cb35-370">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="0cb35-371">Дополнительные сведения о реализации мер безопасности, включая примеры из примера приложения, см. в статье [Передача файлов в ASP.NET Core](#validation).</span><span class="sxs-lookup"><span data-stu-id="0cb35-371">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="0cb35-372">Сценарии использования хранилища</span><span class="sxs-lookup"><span data-stu-id="0cb35-372">Storage scenarios</span></span>

<span data-ttu-id="0cb35-373">К общим вариантам хранилища файлов относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="0cb35-373">Common storage options for files include:</span></span>

* <span data-ttu-id="0cb35-374">База данных</span><span class="sxs-lookup"><span data-stu-id="0cb35-374">Database</span></span>

  * <span data-ttu-id="0cb35-375">В случае отправки небольших файлов база данных часто работает быстрее, чем физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="0cb35-375">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="0cb35-376">База данных часто более удобна по сравнению с вариантами физического хранилища, так как получение записи из базы пользовательских данных может одновременно предоставить содержимое файла (например, изображение аватара).</span><span class="sxs-lookup"><span data-stu-id="0cb35-376">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="0cb35-377">Эксплуатация базы данных потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-377">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="0cb35-378">Физическое хранилище (файловая система или сетевая папка).</span><span class="sxs-lookup"><span data-stu-id="0cb35-378">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="0cb35-379">Обработка передачи больших файлов:</span><span class="sxs-lookup"><span data-stu-id="0cb35-379">For large file uploads:</span></span>
    * <span data-ttu-id="0cb35-380">Ограничения базы данных могут ограничивать размер передачи.</span><span class="sxs-lookup"><span data-stu-id="0cb35-380">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="0cb35-381">Физическое хранилище часто менее экономически выгодно, чем хранилище в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-381">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="0cb35-382">Эксплуатация физического хранилища потенциально дешевле, чем использование службы хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-382">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="0cb35-383">Процесс приложения должен иметь разрешения на чтение и запись для места хранения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-383">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="0cb35-384">**Никогда не предоставляйте разрешение на выполнение.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-384">**Never grant execute permission.**</span></span>

* <span data-ttu-id="0cb35-385">Служба хранилища данных (например, хранилище [BLOB-объектов Azure](https://azure.microsoft.com/services/storage/blobs/)).</span><span class="sxs-lookup"><span data-stu-id="0cb35-385">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="0cb35-386">Обычно службы обеспечивают улучшенную масштабируемость и устойчивость по сравнению с локальными решениями, которые обычно подвержены единым точкам отказа.</span><span class="sxs-lookup"><span data-stu-id="0cb35-386">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="0cb35-387">Затраты на использование служб обычно ниже в сценариях с крупномасштабной инфраструктурой хранения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-387">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="0cb35-388">Дополнительные сведения см. в разделе [Краткое руководство. Использование .NET для создания большого двоичного объекта в хранилище объектов](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="0cb35-388">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="0cb35-389">В этом разделе показан метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, но метод <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> можно использовать для сохранения <xref:System.IO.FileStream> в хранилище BLOB-объектов при работе с <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-389">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="0cb35-390">Сценарии передачи файлов</span><span class="sxs-lookup"><span data-stu-id="0cb35-390">File upload scenarios</span></span>

<span data-ttu-id="0cb35-391">Есть два распространенных подхода к передаче файлов — буферизация и потоковая передача.</span><span class="sxs-lookup"><span data-stu-id="0cb35-391">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="0cb35-392">**Буферизация**</span><span class="sxs-lookup"><span data-stu-id="0cb35-392">**Buffering**</span></span>

<span data-ttu-id="0cb35-393">Весь файл считывается в <xref:Microsoft.AspNetCore.Http.IFormFile> (представление файла на C#, используемого для обработки или сохранения файла).</span><span class="sxs-lookup"><span data-stu-id="0cb35-393">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="0cb35-394">Потребление ресурсов (диска, памяти) при передаче файлов зависит от количества и размера одновременно передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-394">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="0cb35-395">При попытке приложения поместить в буфер слишком много файлов может произойти аварийное завершение работы сайта из-за нехватки памяти или места на диске.</span><span class="sxs-lookup"><span data-stu-id="0cb35-395">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="0cb35-396">Если размер или частота отправки файлов исчерпывают ресурсы приложения, используйте потоковую передачу.</span><span class="sxs-lookup"><span data-stu-id="0cb35-396">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="0cb35-397">Один буферизованный файл размером свыше 64 КБ перемещается из памяти во временный файл на диске.</span><span class="sxs-lookup"><span data-stu-id="0cb35-397">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="0cb35-398">Буферизация небольших файлов описана в следующих разделах этой статьи:</span><span class="sxs-lookup"><span data-stu-id="0cb35-398">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="0cb35-399">Физическое хранилище</span><span class="sxs-lookup"><span data-stu-id="0cb35-399">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="0cb35-400">База данных</span><span class="sxs-lookup"><span data-stu-id="0cb35-400">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="0cb35-401">**Потоковая передача**</span><span class="sxs-lookup"><span data-stu-id="0cb35-401">**Streaming**</span></span>

<span data-ttu-id="0cb35-402">Файл можно получить с помощью составного запроса. Затем он обрабатывается или сохраняется приложением напрямую.</span><span class="sxs-lookup"><span data-stu-id="0cb35-402">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="0cb35-403">Потоковая передача повышает производительность не значительно.</span><span class="sxs-lookup"><span data-stu-id="0cb35-403">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="0cb35-404">При отправке файлов потоковая передача снижает нагрузку на память или на место на диске.</span><span class="sxs-lookup"><span data-stu-id="0cb35-404">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="0cb35-405">Потоковая передача больших файлов рассматривается в разделе [Передача больших файлов с помощью потоковой передачи](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0cb35-405">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="0cb35-406">Передача небольших файлов с привязкой буферизованной модели к физическому хранилищу</span><span class="sxs-lookup"><span data-stu-id="0cb35-406">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="0cb35-407">Для передачи небольших файлов можно применить составную форму или сформировать запрос POST на языке JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cb35-407">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="0cb35-408">В следующем примере демонстрируется использование формы Razor Pages для передачи одного файла (*Pages/BufferedSingleFileUploadPhysical.cshtml* в примере приложения):</span><span class="sxs-lookup"><span data-stu-id="0cb35-408">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="0cb35-409">Следующий пример аналогичен предыдущему примеру, за исключением следующего:</span><span class="sxs-lookup"><span data-stu-id="0cb35-409">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="0cb35-410">JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) используется для отправки данных формы.</span><span class="sxs-lookup"><span data-stu-id="0cb35-410">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="0cb35-411">Проверка не выполняется.</span><span class="sxs-lookup"><span data-stu-id="0cb35-411">There's no validation.</span></span>

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

<span data-ttu-id="0cb35-412">Для выполнения отправки формы в JavaScript для клиентов, которые [не поддерживают Fetch API](https://caniuse.com/#feat=fetch), используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="0cb35-412">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="0cb35-413">Используйте функцию Fetch Polyfill (например, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="0cb35-413">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="0cb35-414">Используйте ключевое слово `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-414">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="0cb35-415">Например:</span><span class="sxs-lookup"><span data-stu-id="0cb35-415">For example:</span></span>

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

<span data-ttu-id="0cb35-416">Для поддержки передачи файлов в HTML-формах должен указываться тип кодировки `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-416">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="0cb35-417">Для элемента ввода `files`, поддерживающего отправку нескольких файлов, в элементе `<input>` необходимо указать атрибут `multiple`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-417">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="0cb35-418">Доступ к отдельным файлам, переданным на сервер, можно получать посредством [привязки модели](xref:mvc/models/model-binding) с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-418">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="0cb35-419">В примере приложения показано несколько отправок буферизованных файлов для баз данных и физических хранилищ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-419">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-420">Свойство `FileName` параметра <xref:Microsoft.AspNetCore.Http.IFormFile> требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-420">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="0cb35-421">Свойство `FileName` следует использовать только в целях вывода и только после HTML-кодирования значения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-421">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="0cb35-422">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="0cb35-422">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="0cb35-423">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="0cb35-423">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="0cb35-424">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="0cb35-424">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="0cb35-425">Проверка</span><span class="sxs-lookup"><span data-stu-id="0cb35-425">Validation</span></span>](#validation)

<span data-ttu-id="0cb35-426">При отправке файлов с помощью привязки модели и <xref:Microsoft.AspNetCore.Http.IFormFile> метод действия может принимать следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="0cb35-426">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="0cb35-427">Один файл <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-427">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="0cb35-428">Любая из следующих коллекций, представляющих несколько файлов:</span><span class="sxs-lookup"><span data-stu-id="0cb35-428">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="0cb35-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="0cb35-430">Привязка сопоставляет файлы форм по имени.</span><span class="sxs-lookup"><span data-stu-id="0cb35-430">Binding matches form files by name.</span></span> <span data-ttu-id="0cb35-431">Например, значение HTML `name` в `<input type="file" name="formFile">` должно соответствовать привязанному к C# параметру или свойству (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="0cb35-431">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="0cb35-432">Дополнительные сведения см. в разделе [Сопоставление значения атрибута имени и имени параметра метода POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="0cb35-432">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="0cb35-433">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="0cb35-433">The following example:</span></span>

* <span data-ttu-id="0cb35-434">Циклично отправляет один или несколько передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-434">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="0cb35-435">Использует метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы вернуть полный путь к файлу, включая его имя.</span><span class="sxs-lookup"><span data-stu-id="0cb35-435">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="0cb35-436">Сохраняет файлы в локальную файловую систему, используя имя файла, созданное приложением.</span><span class="sxs-lookup"><span data-stu-id="0cb35-436">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="0cb35-437">Возвращает общее число и размер отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-437">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="0cb35-438">Чтобы создать имя файла без пути, используйте `Path.GetRandomFileName`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-438">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="0cb35-439">В следующем примере путь получен из конфигурации:</span><span class="sxs-lookup"><span data-stu-id="0cb35-439">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="0cb35-440">Передаваемый в <xref:System.IO.FileStream> путь *должен* содержать имя файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-440">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="0cb35-441">Если имя файла не указано, в среде выполнения возникает исключение <xref:System.UnauthorizedAccessException>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-441">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="0cb35-442">Файлы, передаваемые с помощью интерфейса <xref:Microsoft.AspNetCore.Http.IFormFile>, буферизуются в памяти или на диске на сервере перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="0cb35-442">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="0cb35-443">Внутри метода действия содержимое <xref:Microsoft.AspNetCore.Http.IFormFile> доступно в виде <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-443">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="0cb35-444">Помимо локальной файловой системы, файлы можно сохранять в сетевой папке или в службе хранилища файлов, например в хранилище [BLOB-объектов Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="0cb35-444">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="0cb35-445">Еще один пример, который перебирает несколько файлов для отправки и использует надежные имена файлов, см. в файле *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-445">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-446">Метод [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) вызывает исключение <xref:System.IO.IOException> в случае создания более чем 65 535 файлов без удаления предыдущих временных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-446">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="0cb35-447">Ограничение в 65 535 файлов предусмотрено для каждого сервера.</span><span class="sxs-lookup"><span data-stu-id="0cb35-447">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="0cb35-448">Дополнительные сведения об этом ограничении в ОС Windows см. в примечаниях в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="0cb35-448">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * <span data-ttu-id="0cb35-449">[Функция GetTempFileNameA](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks);</span><span class="sxs-lookup"><span data-stu-id="0cb35-449">[GetTempFileNameA function](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)</span></span>
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="0cb35-450">Передача небольших файлов с привязкой буферизованной модели к базе данных</span><span class="sxs-lookup"><span data-stu-id="0cb35-450">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="0cb35-451">Для сохранения данных двоичных файлов в базе данных с помощью [Entity Framework](/ef/core/index) определите для сущности свойство массива <xref:System.Byte>:</span><span class="sxs-lookup"><span data-stu-id="0cb35-451">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="0cb35-452">Укажите свойство модели страницы для класса, который содержит <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="0cb35-452">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="0cb35-453"><xref:Microsoft.AspNetCore.Http.IFormFile> можно использовать непосредственно как параметр метода действия или свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-453"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="0cb35-454">В предыдущем примере используется свойство модели привязки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-454">The prior example uses a bound model property.</span></span>

<span data-ttu-id="0cb35-455">В форме Razor Pages используется `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-455">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="0cb35-456">При публикации формы на сервере скопируйте <xref:Microsoft.AspNetCore.Http.IFormFile> в поток и сохраните его в базе данных в виде массива байтов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-456">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="0cb35-457">В следующем примере `_dbContext` сохраняет контекст базы данных приложения:</span><span class="sxs-lookup"><span data-stu-id="0cb35-457">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="0cb35-458">Предыдущий пример похож на сценарий, продемонстрированный в примере приложения:</span><span class="sxs-lookup"><span data-stu-id="0cb35-458">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="0cb35-459">*Pages/BufferedSingleFileUploadDb.cshtml*;</span><span class="sxs-lookup"><span data-stu-id="0cb35-459">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="0cb35-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-461">При сохранении двоичных данных в реляционных базах данных следует соблюдать осторожность, так как это может отрицательно сказаться на производительности.</span><span class="sxs-lookup"><span data-stu-id="0cb35-461">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="0cb35-462">Свойство `FileName` параметра <xref:Microsoft.AspNetCore.Http.IFormFile> требует обязательной проверки.</span><span class="sxs-lookup"><span data-stu-id="0cb35-462">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="0cb35-463">Свойство `FileName` следует использовать только в целях вывода и только после HTML-кодирования.</span><span class="sxs-lookup"><span data-stu-id="0cb35-463">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="0cb35-464">В приведенных выше примерах не учитываются вопросы безопасности.</span><span class="sxs-lookup"><span data-stu-id="0cb35-464">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="0cb35-465">Дополнительные сведения приведены в следующих разделах и в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/).</span><span class="sxs-lookup"><span data-stu-id="0cb35-465">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="0cb35-466">Вопросы безопасности</span><span class="sxs-lookup"><span data-stu-id="0cb35-466">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="0cb35-467">Проверка</span><span class="sxs-lookup"><span data-stu-id="0cb35-467">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="0cb35-468">Передача больших файлов с помощью потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="0cb35-468">Upload large files with streaming</span></span>

<span data-ttu-id="0cb35-469">В приведенном ниже примере демонстрируется использование JavaScript для потоковой передачи файла в действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="0cb35-469">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="0cb35-470">Токен против подделки файла создается с помощью пользовательского атрибута фильтра и передается в заголовках HTTP клиента, а не в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="0cb35-470">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="0cb35-471">Так как метод действия обрабатывает передаваемые данные напрямую, привязка модели формы отключается другим пользовательским фильтром.</span><span class="sxs-lookup"><span data-stu-id="0cb35-471">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="0cb35-472">Внутри действия содержимое формы считывается с помощью объекта `MultipartReader`, который считывает каждый объект `MultipartSection` по отдельности, обрабатывая файл или сохраняя содержимое.</span><span class="sxs-lookup"><span data-stu-id="0cb35-472">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="0cb35-473">После считывания составных разделов действие выполняет собственную привязку модели.</span><span class="sxs-lookup"><span data-stu-id="0cb35-473">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="0cb35-474">Вначале страница загружает форму и сохраняет токен против подделки в файле cookie (с помощью атрибута `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="0cb35-474">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="0cb35-475">Атрибут использует встроенную поддержку [защиты от подделки](xref:security/anti-request-forgery) в ASP.NET Core, чтобы задать файл cookie с токеном запроса.</span><span class="sxs-lookup"><span data-stu-id="0cb35-475">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="0cb35-476">Для отключения привязки модели используется `DisableFormValueModelBindingAttribute`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-476">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="0cb35-477">В примере приложения `GenerateAntiforgeryTokenCookieAttribute` и `DisableFormValueModelBindingAttribute` применяются в качестве фильтров к моделям приложений на странице `/StreamedSingleFileUploadDb` и `/StreamedSingleFileUploadPhysical` в `Startup.ConfigureServices` с использованием [соглашений Razor Pages](xref:razor-pages/razor-pages-conventions).</span><span class="sxs-lookup"><span data-stu-id="0cb35-477">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="0cb35-478">Так как привязка модели не считывает форму, параметры, привязанные из формы, не привязываются (запросы, маршруты и заголовки продолжают работать).</span><span class="sxs-lookup"><span data-stu-id="0cb35-478">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="0cb35-479">Метод действия работает напрямую со свойством `Request`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-479">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="0cb35-480">Для считывания каждого раздела служит объект `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-480">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="0cb35-481">Данные "ключ — значение" хранятся в `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-481">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="0cb35-482">После считывания составных разделов содержимое `KeyValueAccumulator` используется для привязки данных формы к типу модели.</span><span class="sxs-lookup"><span data-stu-id="0cb35-482">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="0cb35-483">Полный метод `StreamingController.UploadDatabase` для потоковой передачи в базу данных с EF Core:</span><span class="sxs-lookup"><span data-stu-id="0cb35-483">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="0cb35-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="0cb35-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="0cb35-485">Полный метод `StreamingController.UploadPhysical` для потоковой передачи в физическое расположение:</span><span class="sxs-lookup"><span data-stu-id="0cb35-485">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="0cb35-486">В примере приложения проверки обрабатываются с помощью `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-486">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="0cb35-487">Проверка</span><span class="sxs-lookup"><span data-stu-id="0cb35-487">Validation</span></span>

<span data-ttu-id="0cb35-488">В классе `FileHelpers` примера приложения демонстрируется несколько проверок буферизованного файла <xref:Microsoft.AspNetCore.Http.IFormFile> и потоковой передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-488">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="0cb35-489">Сведения об обработке буферизованных файлов <xref:Microsoft.AspNetCore.Http.IFormFile> в примере приложения см. в описании метода `ProcessFormFile` в файле *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-489">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="0cb35-490">Сведения об обработке потоковых файлов см. в описании метода `ProcessStreamedFile` в том же файле.</span><span class="sxs-lookup"><span data-stu-id="0cb35-490">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="0cb35-491">Методы обработки проверки, показанные в примере приложения, не проверяют содержимое отправленных файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-491">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="0cb35-492">В большинстве рабочих сценариев в файле применяется API сканирования на наличие вирусов и вредоносных программ, прежде чем сделать файл доступным для пользователей или других систем.</span><span class="sxs-lookup"><span data-stu-id="0cb35-492">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="0cb35-493">Хотя пример в разделе содержит рабочий пример методов проверки, не следует реализовывать класс `FileHelpers` в рабочем приложении, кроме таких случаев:</span><span class="sxs-lookup"><span data-stu-id="0cb35-493">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="0cb35-494">Вы полностью разбираетесь в реализации.</span><span class="sxs-lookup"><span data-stu-id="0cb35-494">Fully understand the implementation.</span></span>
> * <span data-ttu-id="0cb35-495">Вы изменяете реализацию соответствующим образом для среды и спецификаций приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-495">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="0cb35-496">**Никогда не реализуйте код безопасности в приложении, не выполнив эти требования.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-496">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="0cb35-497">Проверка содержимого</span><span class="sxs-lookup"><span data-stu-id="0cb35-497">Content validation</span></span>

<span data-ttu-id="0cb35-498">**Используйте сторонний API сканирования на наличие вирусов и вредоносных программ для отправленного содержимого.**</span><span class="sxs-lookup"><span data-stu-id="0cb35-498">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="0cb35-499">Сканирование файлов требует использования ресурсов сервера в сценариях с большими объемами данных.</span><span class="sxs-lookup"><span data-stu-id="0cb35-499">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="0cb35-500">Если производительность обработки запросов снижается из-за сканирования файлов, рассмотрите возможность разгрузки сканирования путем использования [фоновой службы](xref:fundamentals/host/hosted-services), возможно, службы, которая работает на сервере, отличном от сервера приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-500">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="0cb35-501">Как правило, передаваемые файлы хранятся в карантинной области до тех пор, пока фоновый сканер не проверит их на наличие вирусов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-501">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="0cb35-502">При передаче файл перемещается в нормальное место хранения файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-502">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="0cb35-503">Эти действия обычно выполняются вместе с записью базы данных, которая указывает состояние сканирования файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-503">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="0cb35-504">Благодаря такому подходу приложение и сервер приложений остаются в режиме реагирования на запросы.</span><span class="sxs-lookup"><span data-stu-id="0cb35-504">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="0cb35-505">Проверка расширения файла</span><span class="sxs-lookup"><span data-stu-id="0cb35-505">File extension validation</span></span>

<span data-ttu-id="0cb35-506">Расширение переданного файла должно проверяться в соответствии со списком разрешенных расширений.</span><span class="sxs-lookup"><span data-stu-id="0cb35-506">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="0cb35-507">Например:</span><span class="sxs-lookup"><span data-stu-id="0cb35-507">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="0cb35-508">Проверка подписи файла</span><span class="sxs-lookup"><span data-stu-id="0cb35-508">File signature validation</span></span>

<span data-ttu-id="0cb35-509">Подпись файла определяется по первым нескольким байтам в начале файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-509">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="0cb35-510">Эти байты можно использовать, чтобы указать, совпадает ли расширение с содержимым файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-510">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="0cb35-511">Пример приложения проверяет подписи файлов на соответствие нескольким распространенным типам файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-511">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="0cb35-512">В следующем примере проверяется подпись файла для изображения JPEG:</span><span class="sxs-lookup"><span data-stu-id="0cb35-512">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="0cb35-513">Дополнительные сведения о получении дополнительных подписей файлов см. [по этой ссылке](https://www.filesignatures.net/) и в официальных спецификациях файла.</span><span class="sxs-lookup"><span data-stu-id="0cb35-513">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="0cb35-514">Безопасность имени файла</span><span class="sxs-lookup"><span data-stu-id="0cb35-514">File name security</span></span>

<span data-ttu-id="0cb35-515">Никогда не используйте для хранения файла в физическом хранилище имя, предоставляемое клиентом.</span><span class="sxs-lookup"><span data-stu-id="0cb35-515">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="0cb35-516">Создайте надежное имя файла с помощью [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) или [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*), чтобы получить полный путь (включая имя файла) для временного хранилища.</span><span class="sxs-lookup"><span data-stu-id="0cb35-516">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="0cb35-517">Razor автоматически кодирует в HTML значения свойств для вывода.</span><span class="sxs-lookup"><span data-stu-id="0cb35-517">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="0cb35-518">Ниже приведен безопасный код, который можно использовать.</span><span class="sxs-lookup"><span data-stu-id="0cb35-518">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="0cb35-519">Вне Razor содержимое имени файла из запроса пользователя всегда кодируется в <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="0cb35-519">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="0cb35-520">Во многих реализациях следует включать проверку существования файла. В противном случае файл перезаписывается файлом с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="0cb35-520">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="0cb35-521">Предоставьте дополнительную логику для соответствия спецификациям приложения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-521">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="0cb35-522">Проверка размера</span><span class="sxs-lookup"><span data-stu-id="0cb35-522">Size validation</span></span>

<span data-ttu-id="0cb35-523">Ограничьте размер передаваемых файлов.</span><span class="sxs-lookup"><span data-stu-id="0cb35-523">Limit the size of uploaded files.</span></span>

<span data-ttu-id="0cb35-524">В примере приложения размер файла ограничен 2 МБ (указывается в байтах).</span><span class="sxs-lookup"><span data-stu-id="0cb35-524">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="0cb35-525">Ограничение предоставляется с помощью [конфигурации](xref:fundamentals/configuration/index) из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0cb35-525">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="0cb35-526">В классы `PageModel` внедряется `FileSizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-526">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="0cb35-527">Если размер файла превышает ограничение, файл отклоняется:</span><span class="sxs-lookup"><span data-stu-id="0cb35-527">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="0cb35-528">Сопоставление значения атрибута имени и имени параметра метода POST</span><span class="sxs-lookup"><span data-stu-id="0cb35-528">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="0cb35-529">В формах, не относящихся к Razor, которые передают данные формы или используют `FormData` JavaScript напрямую, имя, указанное в элементе формы или `FormData`, должно соответствовать имени параметра в действии контроллера.</span><span class="sxs-lookup"><span data-stu-id="0cb35-529">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="0cb35-530">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0cb35-530">In the following example:</span></span>

* <span data-ttu-id="0cb35-531">При использовании элемента `<input>` атрибуту `name` присваивается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-531">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="0cb35-532">При использовании `FormData` в JavaScript для имени задается значение `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-532">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="0cb35-533">Используйте соответствующее имя для параметра метода C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="0cb35-533">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="0cb35-534">Для метода обработчика страницы Razor Pages с именем `Upload`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-534">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="0cb35-535">Для метода действия контроллера POST приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="0cb35-535">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="0cb35-536">Конфигурация сервера и приложения</span><span class="sxs-lookup"><span data-stu-id="0cb35-536">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="0cb35-537">Ограничение длины составного текста</span><span class="sxs-lookup"><span data-stu-id="0cb35-537">Multipart body length limit</span></span>

<span data-ttu-id="0cb35-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> устанавливает ограничение длины каждого составного текста.</span><span class="sxs-lookup"><span data-stu-id="0cb35-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="0cb35-539">Разделы формы, превышающие это ограничение, вызовут <xref:System.IO.InvalidDataException> при синтаксическом анализе.</span><span class="sxs-lookup"><span data-stu-id="0cb35-539">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="0cb35-540">Значение по умолчанию — 134 217 728 байт (128 МБ).</span><span class="sxs-lookup"><span data-stu-id="0cb35-540">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="0cb35-541">Настройте ограничение с помощью параметра <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-541">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="0cb35-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> используется для настройки <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="0cb35-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="0cb35-543">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-543">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="0cb35-544">В приложении Razor Pages или MVC примените фильтр к модели страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="0cb35-544">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="0cb35-545">Максимальный размер текста запроса Kestrel</span><span class="sxs-lookup"><span data-stu-id="0cb35-545">Kestrel maximum request body size</span></span>

<span data-ttu-id="0cb35-546">Для приложений, размещенных в Kestrel, по умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-546">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="0cb35-547">Настройте ограничение с помощью параметра сервера Kestrel [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size):</span><span class="sxs-lookup"><span data-stu-id="0cb35-547">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="0cb35-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> используется для настройки [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) для одной страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="0cb35-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="0cb35-549">В приложении Razor Pages примените фильтр с [соглашением](xref:razor-pages/razor-pages-conventions) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cb35-549">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="0cb35-550">В приложении Razor Pages или MVC примените фильтр к классу обработчика страницы или методу действия:</span><span class="sxs-lookup"><span data-stu-id="0cb35-550">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="0cb35-551">Другие ограничения Kestrel</span><span class="sxs-lookup"><span data-stu-id="0cb35-551">Other Kestrel limits</span></span>

<span data-ttu-id="0cb35-552">Другие ограничения Kestrel могут применяться для приложений, размещенных в Kestrel:</span><span class="sxs-lookup"><span data-stu-id="0cb35-552">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="0cb35-553">Максимальное число клиентских подключений</span><span class="sxs-lookup"><span data-stu-id="0cb35-553">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="0cb35-554">Скорость передачи данных запросов и ответов</span><span class="sxs-lookup"><span data-stu-id="0cb35-554">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="0cb35-555">Ограничение длины содержимого IIS</span><span class="sxs-lookup"><span data-stu-id="0cb35-555">IIS content length limit</span></span>

<span data-ttu-id="0cb35-556">По умолчанию ограничение запроса (`maxAllowedContentLength`) составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-556">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="0cb35-557">Ограничение можно настроить в файле *web.config*:</span><span class="sxs-lookup"><span data-stu-id="0cb35-557">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="0cb35-558">Этот параметр применим только к службам IIS.</span><span class="sxs-lookup"><span data-stu-id="0cb35-558">This setting only applies to IIS.</span></span> <span data-ttu-id="0cb35-559">При размещении в Kestrel такой проблемы возникать не должно.</span><span class="sxs-lookup"><span data-stu-id="0cb35-559">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="0cb35-560">Дополнительные сведения см. в статье [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Ограничения запроса <requestLimits>).</span><span class="sxs-lookup"><span data-stu-id="0cb35-560">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="0cb35-561">Ограничения в модуле ASP.NET Core или наличие модуля фильтрации запросов IIS могут ограничить передачу до 2 или 4 ГБ.</span><span class="sxs-lookup"><span data-stu-id="0cb35-561">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="0cb35-562">Дополнительные сведения см. в [этой ветке форума](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="0cb35-562">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="0cb35-563">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="0cb35-563">Troubleshoot</span></span>

<span data-ttu-id="0cb35-564">Ниже описываются некоторые распространенные проблемы, которые возникают при передаче файлов, и возможные способы их решения.</span><span class="sxs-lookup"><span data-stu-id="0cb35-564">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="0cb35-565">Ошибка "Не найдено" при развертывании на сервере IIS</span><span class="sxs-lookup"><span data-stu-id="0cb35-565">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="0cb35-566">Следующая ошибка свидетельствует о том, что размер передаваемых файлов превышает настроенное на сервере ограничение длины содержимого:</span><span class="sxs-lookup"><span data-stu-id="0cb35-566">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="0cb35-567">Дополнительные сведения об увеличении предела см. в разделе [Ограничение длины содержимого IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="0cb35-567">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="0cb35-568">Сбой подключения</span><span class="sxs-lookup"><span data-stu-id="0cb35-568">Connection failure</span></span>

<span data-ttu-id="0cb35-569">Ошибка соединения и сброс соединения с сервером, скорее всего, означают, что размер отправленного файла превышает максимальную длину текста запроса Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0cb35-569">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="0cb35-570">Дополнительные сведения см. в разделе [Максимальный размер текста запроса Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="0cb35-570">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="0cb35-571">Может также потребоваться настроить ограничения подключений клиентов Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0cb35-571">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="0cb35-572">Исключение, связанное с пустой ссылкой в IFormFile</span><span class="sxs-lookup"><span data-stu-id="0cb35-572">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="0cb35-573">Если контроллер принимает передаваемые файлы с помощью <xref:Microsoft.AspNetCore.Http.IFormFile>, но значение равно `null`, проверьте, указан ли в HTML-форме атрибут `enctype` со значением `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-573">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="0cb35-574">Если этот атрибут не задан для элемента `<form>`, передача файлов происходить не будет и все связанные аргументы <xref:Microsoft.AspNetCore.Http.IFormFile> будут иметь значение `null`.</span><span class="sxs-lookup"><span data-stu-id="0cb35-574">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="0cb35-575">Убедитесь также, что [именование передачи в данных формы совпадает с именованием приложения](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="0cb35-575">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="0cb35-576">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0cb35-576">Additional resources</span></span>

* [<span data-ttu-id="0cb35-577">Неограниченная отправка файлов</span><span class="sxs-lookup"><span data-stu-id="0cb35-577">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="0cb35-578">Безопасность в Azure. Механизм безопасности. Проверки входных данных | Снижение риска</span><span class="sxs-lookup"><span data-stu-id="0cb35-578">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="0cb35-579">Конструктивные шаблоны облачных решений Azure. Шаблон ключа камердинера</span><span class="sxs-lookup"><span data-stu-id="0cb35-579">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
