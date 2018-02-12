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
ms.openlocfilehash: 24eaa0dd9293cc932c51d280300308e835a0840e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="0ba13-103">Отправка файлов на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ba13-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="0ba13-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="0ba13-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0ba13-105">В этом разделе описана отправка файлов с использованием страницы Razor.</span><span class="sxs-lookup"><span data-stu-id="0ba13-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="0ba13-106">[Пример приложения Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) в этом руководстве использует для отправки файлов простую привязку модели, которая хорошо подходит для небольших файлов.</span><span class="sxs-lookup"><span data-stu-id="0ba13-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="0ba13-107">Сведения о потоковой передаче больших файлов см. в статье [Отправка больших файлов с помощью потоковой передачи](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0ba13-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="0ba13-108">В приведенных ниже действиях вы добавляете функцию отправки файлов с расписанием фильмов в пример приложения.</span><span class="sxs-lookup"><span data-stu-id="0ba13-108">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="0ba13-109">Расписание фильмов представлено классом `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="0ba13-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="0ba13-110">Он включает в себя две версии расписания.</span><span class="sxs-lookup"><span data-stu-id="0ba13-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="0ba13-111">Одна версия, `PublicSchedule`, предоставляется клиентам.</span><span class="sxs-lookup"><span data-stu-id="0ba13-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="0ba13-112">Для сотрудников организации используется другая версия — `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="0ba13-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="0ba13-113">Каждая версия отправляется в виде отдельного файла.</span><span class="sxs-lookup"><span data-stu-id="0ba13-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="0ba13-114">В руководстве показано, как выполнить две отправки файла со страницы на сервер с помощью отдельной операции POST.</span><span class="sxs-lookup"><span data-stu-id="0ba13-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="0ba13-115">Добавление класса FileUpload</span><span class="sxs-lookup"><span data-stu-id="0ba13-115">Add a FileUpload class</span></span>

<span data-ttu-id="0ba13-116">Создайте страницу Razor для передачи пары файлов ниже.</span><span class="sxs-lookup"><span data-stu-id="0ba13-116">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="0ba13-117">Добавьте класс `FileUpload`, привязанный к странице для получения данных расписания.</span><span class="sxs-lookup"><span data-stu-id="0ba13-117">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="0ba13-118">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-118">Right click the *Models* folder.</span></span> <span data-ttu-id="0ba13-119">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="0ba13-119">Select **Add** > **Class**.</span></span> <span data-ttu-id="0ba13-120">Назовите класс **FileUpload** и добавьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="0ba13-120">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="0ba13-121">Этот класс содержит свойство для заголовка расписания и свойство для каждой из двух версий расписания.</span><span class="sxs-lookup"><span data-stu-id="0ba13-121">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="0ba13-122">Все три свойства являются обязательными, а заголовок должен иметь длину от 3 до 60 символов.</span><span class="sxs-lookup"><span data-stu-id="0ba13-122">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="0ba13-123">Добавление вспомогательного метода для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="0ba13-123">Add a helper method to upload files</span></span>

<span data-ttu-id="0ba13-124">Чтобы избежать дублирования кода для обработки отправленных файлов расписания, сначала добавьте статический вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="0ba13-124">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="0ba13-125">Создайте папку *Utilities* в приложении и добавьте файл *FileHelpers.cs* с приведенным ниже содержимым.</span><span class="sxs-lookup"><span data-stu-id="0ba13-125">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="0ba13-126">Вспомогательный метод `ProcessFormFile` принимает [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) и возвращает строку, содержащую размер и содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="0ba13-126">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="0ba13-127">Выполняется проверка типа содержимого и длины.</span><span class="sxs-lookup"><span data-stu-id="0ba13-127">The content type and length are checked.</span></span> <span data-ttu-id="0ba13-128">Если файл не проходит проверку, в `ModelState` добавляется ошибка.</span><span class="sxs-lookup"><span data-stu-id="0ba13-128">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="0ba13-129">Добавление класса Schedule</span><span class="sxs-lookup"><span data-stu-id="0ba13-129">Add the Schedule class</span></span>

<span data-ttu-id="0ba13-130">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-130">Right click the *Models* folder.</span></span> <span data-ttu-id="0ba13-131">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="0ba13-131">Select **Add** > **Class**.</span></span> <span data-ttu-id="0ba13-132">Назовите класс **Schedule** и добавьте следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="0ba13-132">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="0ba13-133">Класс использует атрибуты `Display` и `DisplayFormat`, которые создают понятные заголовки и форматирование при отрисовке данных расписания.</span><span class="sxs-lookup"><span data-stu-id="0ba13-133">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="0ba13-134">Обновление MovieContext</span><span class="sxs-lookup"><span data-stu-id="0ba13-134">Update the MovieContext</span></span>

<span data-ttu-id="0ba13-135">Укажите `DbSet` в `MovieContext` (*Models/MovieContext.cs*) для расписаний:</span><span class="sxs-lookup"><span data-stu-id="0ba13-135">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="0ba13-136">Добавление таблицы Schedule в базу данных</span><span class="sxs-lookup"><span data-stu-id="0ba13-136">Add the Schedule table to the database</span></span>

<span data-ttu-id="0ba13-137">Откройте консоль диспетчера пакетов (PMC): **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0ba13-137">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0ba13-139">В PMC выполните указанные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0ba13-139">In the PMC, execute the following commands.</span></span> <span data-ttu-id="0ba13-140">Эти команды добавляют таблицу `Schedule` в базу данных.</span><span class="sxs-lookup"><span data-stu-id="0ba13-140">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="0ba13-141">Добавление страницы Razor для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="0ba13-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="0ba13-142">В папке *Pages* создайте папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="0ba13-143">В папке *Schedules* создайте страницу *Index.cshtml* для отправки расписания со следующим содержимым.</span><span class="sxs-lookup"><span data-stu-id="0ba13-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="0ba13-144">Каждая группа формы включает метку **\<label>**, отображающую имя каждого свойства класса.</span><span class="sxs-lookup"><span data-stu-id="0ba13-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="0ba13-145">Атрибуты `Display` в модели `FileUpload` предоставляют отображаемые значения для меток.</span><span class="sxs-lookup"><span data-stu-id="0ba13-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="0ba13-146">Например, отображаемое имя свойства `UploadPublicSchedule` задается с помощью `[Display(Name="Public Schedule")]`, в результате чего при отрисовке формы в метке отображается текст "Public Schedule" (Общее расписание).</span><span class="sxs-lookup"><span data-stu-id="0ba13-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="0ba13-147">Каждая группа формы включает период проверки **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="0ba13-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="0ba13-148">Если введенные пользователем данные не соответствуют атрибутам свойства, заданным в классе `FileUpload`, или какой-либо из этапов проверки файла в методе `ProcessFormFile` завершается с ошибкой, модель не проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="0ba13-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="0ba13-149">При сбое проверки модели для пользователя выводится сообщение с полезными данными.</span><span class="sxs-lookup"><span data-stu-id="0ba13-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="0ba13-150">Например, для свойства `Title` указаны `[Required]` и `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="0ba13-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="0ba13-151">Если пользователь не задает заголовок, он получает сообщение, указывающее на обязательный характер этого значения.</span><span class="sxs-lookup"><span data-stu-id="0ba13-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="0ba13-152">Если пользователь вводит значение длиной менее 3 или более 60 символов, он получает сообщение о неправильной длине.</span><span class="sxs-lookup"><span data-stu-id="0ba13-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="0ba13-153">Если указан файл без содержимого, появляется сообщение о том, что этот файл пуст.</span><span class="sxs-lookup"><span data-stu-id="0ba13-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="0ba13-154">Добавление страничной модели</span><span class="sxs-lookup"><span data-stu-id="0ba13-154">Add the page model</span></span>

<span data-ttu-id="0ba13-155">Добавьте страничную модель (*Index.cshtml.cs*) в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-155">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="0ba13-156">Модель страницы (`IndexModel` в *Index.cshtml.cs*) привязывается к классу `FileUpload`.</span><span class="sxs-lookup"><span data-stu-id="0ba13-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0ba13-157">Модель также использует список расписаний (`IList<Schedule>`) для отображения расписаний, хранящихся в базе данных на странице.</span><span class="sxs-lookup"><span data-stu-id="0ba13-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0ba13-158">Когда страница загружается с `OnGetAsync`, `Schedules` заполняется значениями из базы данных и используется для создания HTML-таблицы загруженных расписаний.</span><span class="sxs-lookup"><span data-stu-id="0ba13-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="0ba13-159">При публикации формы на сервере проверяется `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0ba13-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="0ba13-160">В случае ошибки `Schedule` перестраивается, а страница отрисовывается с отображением одного сообщения или нескольких о том, почему не удалось выполнить проверку страницы.</span><span class="sxs-lookup"><span data-stu-id="0ba13-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="0ba13-161">При прохождении проверки свойства `FileUpload` используются в *OnPostAsync*, чтобы передать файлы для двух версий расписания и создать объект `Schedule` для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="0ba13-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="0ba13-162">После этого расписание сохраняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0ba13-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="0ba13-163">Ссылка на страницу Razor для отправки файлов</span><span class="sxs-lookup"><span data-stu-id="0ba13-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="0ba13-164">Откройте *_Layout.cshtml* и добавьте ссылку на панель навигации, позволяющую перейти на страницу отправки файлов.</span><span class="sxs-lookup"><span data-stu-id="0ba13-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="0ba13-165">Добавление страницы для подтверждения удаления расписания</span><span class="sxs-lookup"><span data-stu-id="0ba13-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="0ba13-166">Когда пользователь запускает операцию удаления расписания, нужно предусмотреть возможность ее отмены.</span><span class="sxs-lookup"><span data-stu-id="0ba13-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="0ba13-167">Добавьте страницу подтверждения удаления (*Delete.cshtml*) в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="0ba13-168">Страничная модель (*Delete.cshtml.cs*) загружает отдельное расписание, определяемое идентификатором `id` в данных маршрута запроса.</span><span class="sxs-lookup"><span data-stu-id="0ba13-168">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="0ba13-169">Добавьте файл *Delete.cshtml.cs* в папку *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="0ba13-170">Метод `OnPostAsync` обрабатывает удаление расписания по его `id`.</span><span class="sxs-lookup"><span data-stu-id="0ba13-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="0ba13-171">После успешного удаления расписания `RedirectToPage` направляет пользователя обратно на страницу расписаний *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0ba13-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="0ba13-172">Рабочая страница Razor для расписаний</span><span class="sxs-lookup"><span data-stu-id="0ba13-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="0ba13-173">При загрузке страницы метки и входные данные для заголовка расписания, общедоступного расписания и частного расписания отрисовываются с помощью кнопки отправки.</span><span class="sxs-lookup"><span data-stu-id="0ba13-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Страница Razor в том виде, в котором она отображается при начальной загрузке, без ошибок проверки и пустых полей](uploading-files/_static/browser1.png)

<span data-ttu-id="0ba13-175">Нажатие кнопки **Upload** (Отправить) без заполнения полей нарушает атрибуты `[Required]` в модели.</span><span class="sxs-lookup"><span data-stu-id="0ba13-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="0ba13-176">`ModelState` не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="0ba13-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="0ba13-177">Для пользователя отображаются сообщения об ошибках проверки</span><span class="sxs-lookup"><span data-stu-id="0ba13-177">The validation error messages are displayed to the user:</span></span>

![Сообщения об ошибках проверки отображаются рядом с каждым элементом управления ввода.](uploading-files/_static/browser2.png)

<span data-ttu-id="0ba13-179">Введите две буквы в поле **Title** (Заголовок).</span><span class="sxs-lookup"><span data-stu-id="0ba13-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="0ba13-180">Сообщение о проверке изменяется, указывая, что заголовок должен иметь длину от 3 до 60 символов.</span><span class="sxs-lookup"><span data-stu-id="0ba13-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Изменение сообщения о проверке заголовка](uploading-files/_static/browser3.png)

<span data-ttu-id="0ba13-182">При отправке одного расписания или нескольких они отображаются в разделе **Loaded Schedules** (Загруженные расписания).</span><span class="sxs-lookup"><span data-stu-id="0ba13-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Таблица загруженных расписаний, показывающая заголовок каждого расписания, дату отправки в формате UTC, размер файла общедоступной версии и размер файла закрытой версии](uploading-files/_static/browser4.png)

<span data-ttu-id="0ba13-184">Пользователь может щелкнуть ссылку **Delete** (Удалить), чтобы перейти в представление подтверждения удаления, где сможет подтвердить или отменить операцию удаления.</span><span class="sxs-lookup"><span data-stu-id="0ba13-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0ba13-185">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="0ba13-185">Troubleshooting</span></span>

<span data-ttu-id="0ba13-186">Сведения об устранении неполадок с отправкой `IFormFile` см. в статье [Передача файлов в ASP.NET Core: устранение неполадок](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="0ba13-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="0ba13-187">Благодарим вас за изучение общих сведений о страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="0ba13-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0ba13-188">Мы будем рады любым комментариям.</span><span class="sxs-lookup"><span data-stu-id="0ba13-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="0ba13-189">Отличным дополнением к этому учебнику является статья [Начало работы с EF и MVC](xref:data/ef-mvc/intro).</span><span class="sxs-lookup"><span data-stu-id="0ba13-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ba13-190">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0ba13-190">Additional resources</span></span>

* [<span data-ttu-id="0ba13-191">Передача файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ba13-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="0ba13-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="0ba13-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="0ba13-193">Предыдущая тема. Проверка</span><span class="sxs-lookup"><span data-stu-id="0ba13-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
