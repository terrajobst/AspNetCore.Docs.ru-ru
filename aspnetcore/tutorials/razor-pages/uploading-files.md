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
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>Отправка файлов на страницу Razor в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В этом разделе описана отправка файлов с использованием страницы Razor.

[Пример приложения Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) в этом руководстве использует для отправки файлов простую привязку модели, которая хорошо подходит для небольших файлов. Сведения о потоковой передаче больших файлов см. в статье [Отправка больших файлов с помощью потоковой передачи](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

В приведенных ниже действиях вы добавляете функцию отправки файлов с расписанием фильмов в пример приложения. Расписание фильмов представлено классом `Schedule`. Он включает в себя две версии расписания. Одна версия, `PublicSchedule`, предоставляется клиентам. Для сотрудников организации используется другая версия — `PrivateSchedule`. Каждая версия отправляется в виде отдельного файла. В руководстве показано, как выполнить две отправки файла со страницы на сервер с помощью отдельной операции POST.

## <a name="add-a-fileupload-class"></a>Добавление класса FileUpload

Создайте страницу Razor для передачи пары файлов ниже. Добавьте класс `FileUpload`, привязанный к странице для получения данных расписания. Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Назовите класс **FileUpload** и добавьте следующие свойства.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

Этот класс содержит свойство для заголовка расписания и свойство для каждой из двух версий расписания. Все три свойства являются обязательными, а заголовок должен иметь длину от 3 до 60 символов.

## <a name="add-a-helper-method-to-upload-files"></a>Добавление вспомогательного метода для отправки файлов

Чтобы избежать дублирования кода для обработки отправленных файлов расписания, сначала добавьте статический вспомогательный метод. Создайте папку *Utilities* в приложении и добавьте файл *FileHelpers.cs* с приведенным ниже содержимым. Вспомогательный метод `ProcessFormFile` принимает [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) и возвращает строку, содержащую размер и содержимое файла. Выполняется проверка типа содержимого и длины. Если файл не проходит проверку, в `ModelState` добавляется ошибка.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>Добавление класса Schedule

Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Назовите класс **Schedule** и добавьте следующие свойства.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

Класс использует атрибуты `Display` и `DisplayFormat`, которые создают понятные заголовки и форматирование при отрисовке данных расписания.

## <a name="update-the-moviecontext"></a>Обновление MovieContext

Укажите `DbSet` в `MovieContext` (*Models/MovieContext.cs*) для расписаний:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Добавление таблицы Schedule в базу данных

Откройте консоль диспетчера пакетов (PMC): **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.

![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

В PMC выполните указанные ниже команды. Эти команды добавляют таблицу `Schedule` в базу данных.

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Добавление страницы Razor для отправки файлов

В папке *Pages* создайте папку *Schedules*. В папке *Schedules* создайте страницу *Index.cshtml* для отправки расписания со следующим содержимым.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Каждая группа формы включает метку **\<label>**, отображающую имя каждого свойства класса. Атрибуты `Display` в модели `FileUpload` предоставляют отображаемые значения для меток. Например, отображаемое имя свойства `UploadPublicSchedule` задается с помощью `[Display(Name="Public Schedule")]`, в результате чего при отрисовке формы в метке отображается текст "Public Schedule" (Общее расписание).

Каждая группа формы включает период проверки **\<span>**. Если введенные пользователем данные не соответствуют атрибутам свойства, заданным в классе `FileUpload`, или какой-либо из этапов проверки файла в методе `ProcessFormFile` завершается с ошибкой, модель не проходит проверку. При сбое проверки модели для пользователя выводится сообщение с полезными данными. Например, для свойства `Title` указаны `[Required]` и `[StringLength(60, MinimumLength = 3)]`. Если пользователь не задает заголовок, он получает сообщение, указывающее на обязательный характер этого значения. Если пользователь вводит значение длиной менее 3 или более 60 символов, он получает сообщение о неправильной длине. Если указан файл без содержимого, появляется сообщение о том, что этот файл пуст.

## <a name="add-the-page-model"></a>Добавление страничной модели

Добавьте страничную модель (*Index.cshtml.cs*) в папку *Schedules*.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Модель страницы (`IndexModel` в *Index.cshtml.cs*) привязывается к классу `FileUpload`.

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Модель также использует список расписаний (`IList<Schedule>`) для отображения расписаний, хранящихся в базе данных на странице.

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Когда страница загружается с `OnGetAsync`, `Schedules` заполняется значениями из базы данных и используется для создания HTML-таблицы загруженных расписаний.

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

При публикации формы на сервере проверяется `ModelState`. В случае ошибки `Schedule` перестраивается, а страница отрисовывается с отображением одного сообщения или нескольких о том, почему не удалось выполнить проверку страницы. При прохождении проверки свойства `FileUpload` используются в *OnPostAsync*, чтобы передать файлы для двух версий расписания и создать объект `Schedule` для хранения данных. После этого расписание сохраняется в базе данных.

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Ссылка на страницу Razor для отправки файлов

Откройте *_Layout.cshtml* и добавьте ссылку на панель навигации, позволяющую перейти на страницу отправки файлов.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Добавление страницы для подтверждения удаления расписания

Когда пользователь запускает операцию удаления расписания, нужно предусмотреть возможность ее отмены. Добавьте страницу подтверждения удаления (*Delete.cshtml*) в папку *Schedules*.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Страничная модель (*Delete.cshtml.cs*) загружает отдельное расписание, определяемое идентификатором `id` в данных маршрута запроса. Добавьте файл *Delete.cshtml.cs* в папку *Schedules*.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

Метод `OnPostAsync` обрабатывает удаление расписания по его `id`.

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

После успешного удаления расписания `RedirectToPage` направляет пользователя обратно на страницу расписаний *Index.cshtml*.

## <a name="the-working-schedules-razor-page"></a>Рабочая страница Razor для расписаний

При загрузке страницы метки и входные данные для заголовка расписания, общедоступного расписания и частного расписания отрисовываются с помощью кнопки отправки.

![Страница Razor в том виде, в котором она отображается при начальной загрузке, без ошибок проверки и пустых полей](uploading-files/_static/browser1.png)

Нажатие кнопки **Upload** (Отправить) без заполнения полей нарушает атрибуты `[Required]` в модели. `ModelState` не является допустимым. Для пользователя отображаются сообщения об ошибках проверки

![Сообщения об ошибках проверки отображаются рядом с каждым элементом управления ввода.](uploading-files/_static/browser2.png)

Введите две буквы в поле **Title** (Заголовок). Сообщение о проверке изменяется, указывая, что заголовок должен иметь длину от 3 до 60 символов.

![Изменение сообщения о проверке заголовка](uploading-files/_static/browser3.png)

При отправке одного расписания или нескольких они отображаются в разделе **Loaded Schedules** (Загруженные расписания).

![Таблица загруженных расписаний, показывающая заголовок каждого расписания, дату отправки в формате UTC, размер файла общедоступной версии и размер файла закрытой версии](uploading-files/_static/browser4.png)

Пользователь может щелкнуть ссылку **Delete** (Удалить), чтобы перейти в представление подтверждения удаления, где сможет подтвердить или отменить операцию удаления.

## <a name="troubleshooting"></a>Устранение неполадок

Сведения об устранении неполадок с отправкой `IFormFile` см. в статье [Передача файлов в ASP.NET Core: устранение неполадок](xref:mvc/models/file-uploads#troubleshooting).

Благодарим вас за изучение общих сведений о страницах Razor. Мы будем рады любым комментариям. Отличным дополнением к этому учебнику является статья [Начало работы с EF и MVC](xref:data/ef-mvc/intro).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Передача файлов в ASP.NET Core](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Предыдущая тема. Проверка](xref:tutorials/razor-pages/validation)
