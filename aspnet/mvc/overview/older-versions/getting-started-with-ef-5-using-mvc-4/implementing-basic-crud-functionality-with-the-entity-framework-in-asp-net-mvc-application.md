---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Реализация функциональности основные CRUD с платформой Entity Framework в приложение ASP.NET MVC (часть 2 из 10) | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: acec5c9641b1de230956478c4396d1d541fcb0eb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875332"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Реализация функциональности основные CRUD с платформой Entity Framework в приложение ASP.NET MVC (часть 2 из 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебнике вы создали приложение MVC, хранит и отображает данные с помощью платформы Entity Framework и SQL Server LocalDB. В этом учебнике предстоит просмотреть и настроить CRUD (Создание, чтение, обновление и удаление) код, который MVC функции формирования шаблонов автоматически создает для вас в контроллеры и представления.

> [!NOTE]
> Широко распространена практика реализации шаблона репозитория, позволяющего создать уровень абстракции между контроллером и уровнем доступа к данным. Для простоты в этих учебниках не реализуются репозиторий до более поздней версии учебнике этой серии.


В этом учебнике вы создадите на следующих сайтах:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Создание веб-страницу

Код формирования шаблонов для учащихся `Index` опущены страницы `Enrollments` свойства, так как это свойство содержит коллекцию. В `Details` страницы, как отображать содержимое коллекции в HTML-таблицу.

 В *Controllers\StudentController.cs*, метод действия для `Details` просмотра использует `Find` метод для извлечения одной `Student` сущности. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Значение ключа передается методу в качестве `id` параметр и поступают из данных о маршруте в **сведения** гиперссылки на странице индекса. 

1. Open *Views\Student\Details.cshtml*. Каждое поле отображается с помощью `DisplayFor` вспомогательного приложения, как показано в следующем примере: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. После `EnrollmentDate` и непосредственно перед закрывающим тегом `fieldset` , добавьте код, чтобы отобразить список регистраций, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Этот код циклически обрабатывает сущности в свойстве навигации `Enrollments`. Для каждого `Enrollment` сущности в свойстве, оно отображает название курса и уровень. Название курса извлекается из `Course` сущность, которая хранится в `Course` свойство навигации `Enrollments` сущности. Все эти данные извлекаются из базы данных автоматически при необходимости. (Другими словами, вы используете отложенную загрузку, здесь. Вы не указали *упреждающую* для `Courses` свойство навигации, поэтому первый раз, при попытке доступа к этому свойству, отправляется запрос к базе данных для получения данных. Вы можете прочитать больше о отложенную загрузку и упреждающую в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника далее в этой серии.)
3. Запустите страницу, выбрав **учащихся** и нажав кнопку **сведения** ссылку для Александр Carson. Откроется список курсов и оценок для выбранного учащегося:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Обновление страницы создания

1. В *Controllers\StudentController.cs*, замените `HttpPost``Create` метод действия на следующий код, чтобы добавить `try-catch` блока и [привязки атрибута](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) в метод формирования шаблонов: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Этот код добавляет `Student` сущности, созданные связывателя модели ASP.NET MVC для `Students` набор сущности и затем сохраняет изменения в базе данных. (*Связывателя модели* ссылается на функции ASP.NET MVC, которые делают его проще работать с данными, отправленных в форме; связыватель модели формы учтена преобразует значения с типами среды CLR и передает их в параметры метода действия. In this Case, создает экземпляр связывателя модели `Student` сущности можно с помощью свойства значения из `Form` коллекции.)

    `ValidateAntiForgeryToken` Атрибут помогает предотвратить [межсайтовой подделки запросов](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Запустите страницу, выбрав **учащихся** и нажав кнопку **создать новый**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Некоторые проверки данных выполняется по умолчанию. Введите имена и обнаружении неверной даты и нажмите кнопку **создать** для сообщения об ошибке.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Следующий выделенный код показывает проверки модели.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Измените дату, например 9/1/2005 допустимое значение и нажмите кнопку **создать** для просмотра нового студента, которые отображаются в **индекс** страницы.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Обновление страницы правки POST

В *Controllers\StudentController.cs*, `HttpGet` `Edit` метод (один без `HttpPost` атрибут) использует `Find` метод для извлечения выбранного `Student` сущности, как вы узнали в `Details` метод. Изменять этот метод не нужно.

Тем не менее, замените `HttpPost` `Edit` метод действия на следующий код, чтобы добавить `try-catch` блока и [атрибута Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Этот код подобен видели в `HttpPost` `Create` метод. Однако вместо добавления сущности, созданные связыватель модели для набора сущностей, этот код устанавливает флаг, в сущности, которое указывает, был изменен. Когда [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) вызывается метод, [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) флаг заставляет Entity Framework для создания инструкции SQL для обновления строки базы данных. Все столбцы, строки базы данных будут обновлены, включая те, которые не были изменены пользователем, и учитываются конфликтов параллелизма. (Вы узнаете, как обрабатывать параллелизма позднее в этом учебнике этой серии.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Состояния сущностей и Attach и методы SaveChanges

Контекст базы данных отслеживает состояние синхронизации сущностей в памяти с соответствующими им строками в базе данных. Данные отслеживания определяют, что происходит при вызове метода `SaveChanges`. Например, при передаче новую сущность, [добавить](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) метод, который присваивается состояние сущности `Added`. Затем при вызове [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метода, контекст базы данных выдает SQL `INSERT` команды.

Сущность может находиться в одном из[следующие состояния](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Сущности в базе данных еще не существует. `SaveChanges` Метод должен выдать `INSERT` инструкции.
- `Unchanged`. С этой сущностью не нужно выполнять никакие действия с помощью метода `SaveChanges`. Это начальный статус сущности, который она имеет при чтении из базы данных.
- `Modified`. Были изменены значения некоторых или всех свойств сущности. `SaveChanges` Метод должен выдать `UPDATE` инструкции.
- `Deleted`. Сущность отмечена для удаления. `SaveChanges` Метод должен выдать `DELETE` инструкции.
- `Detached`. Сущность не отслеживается контекстом базы данных.

В классическом приложении изменения состояния обычно осуществляются автоматически. В приложении рабочего стола типа чтение сущности и изменить некоторые значения свойств. В этом случае состояние сущности автоматически изменится на `Modified`. Затем при вызове `SaveChanges`, Entity Framework создает SQL `UPDATE` инструкцию, которая обновляет только фактические свойства, которые были изменены.

Для этой непрерывной последовательности не допускает отключены от веб-приложений. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , читающего сущность удаляется после отрисовки страницы. При `HttpPost` `Edit` вызывается метод действия, новый запрос выполняется, и у вас есть новый экземпляр [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), поэтому необходимо вручную установить состояние сущности `Modified.` , а затем при вызове `SaveChanges`, Платформа Entity Framework обновляет все столбцы, строки базы данных, так как контекст никак не может знать, какие свойства были изменены.

Если требуется, чтобы SQL `Update` инструкцию, чтобы обновить только поля, которые пользователь действительно были изменены, можно сохранить первоначальные значения определенным образом (например, скрытые поля), чтобы они были доступны при `HttpPost` `Edit` вызывается метод. Затем вы можете создать `Student` сущности с помощью исходных значений, вызов `Attach` с этой версией исходной сущности, обновления значений сущности с новыми значениями и затем вызвать `SaveChanges.` Дополнительные сведения см. в разделе [ Состояния сущности и SaveChanges](https://msdn.microsoft.com/data/jj592676) и [локальные данные](https://msdn.microsoft.com/data/jj592872) в центре разработчиков MSDN данных.

Код в *Views\Student\Edit.cshtml* аналогична видели в *Create.cshtml*, и изменения не требуются.

Запустите страницу, выбрав **учащихся** и затем нажав кнопку **изменить** гиперссылки.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Измените определенные данные и нажмите кнопку **Save** (Сохранить). Можно видеть данные, измененные страницы индекса.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы удаления

В *Controllers\StudentController.cs*, код шаблона для `HttpGet` `Delete` использует метод `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` и `Edit` методы. Тем не менее, чтобы реализовать настраиваемое сообщение об ошибке при сбое вызова метода `SaveChanges`, необходимо добавить некоторые функции в этот метод и соответствующее ему представление.

Как и в случае с операциями обновления и создания, операции удаления требуют двух методов действия. Метод, который вызывается в ответ на запрос GET отображает представление, которое дает пользователю возможность утверждения или отменить операцию удаления. Если пользователь подтверждает ее, создается запрос POST. Когда это происходит, `HttpPost` `Delete` вызывается метод, а затем этот метод фактически выполняет операцию удаления.

Вы добавите `try-catch` заблокировать к `HttpPost` `Delete` метод для обработки всех ошибок, которые могут возникнуть при обновлении базы данных. При возникновении ошибки `HttpPost` `Delete` вызовы метода `HttpGet` `Delete` метод, передав ему параметр, указывающий, что произошла ошибка. `HttpGet Delete` Метод затем заново отобразит страницу подтверждения и сообщение об ошибке, предоставляя пользователю возможность отменить или повторить попытку.

1. Замените `HttpGet` `Delete` метод действия с следующий код, который управляет отчеты об ошибках: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Этот код принимает [необязательно](https://msdn.microsoft.com/library/dd264739.aspx) логический параметр, указывающий, является ли он был вызван после сбоя, чтобы сохранить изменения. Этот параметр является `false` при `HttpGet` `Delete` метод вызывается без предыдущего сбоя. При вызове `HttpPost` `Delete` в ответ на ошибку обновления базы данных, параметр является `true` и сообщение об ошибке передается в представление.
2. Замените `HttpPost` `Delete` метода действия (с именем `DeleteConfirmed`) следующим кодом, который выполняет операцию удаления фактическое и перехватывает все ошибки обновления базы данных.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Этот код извлекает выбранную сущность, затем вызывает метод [удалить](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) метод, чтобы задать состояния сущности `Deleted`. Когда `SaveChanges` вызывается SQL `DELETE` команда создается. Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Формирования шаблонов код, называемый `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` метод уникальная сигнатура. (Среда CLR требуется перегруженные методы, чтобы иметь параметры другой метод). Теперь, когда сигнатурах являются уникальными, можно не покидайте соглашение MVC и использовать то же имя для `HttpPost` и `HttpGet` удаления методов.

     Если приоритетом является повышение производительности в приложении большого объема, чтобы избежать ненужных SQL-запрос для получения строки путем замены строки кода, которые вызывают `Find` и `Remove` методов с помощью следующего кода, как показано желтым цветом выделение:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Этот код создает экземпляр `Student` сущности с помощью только значения первичного ключа, а затем задает состояние сущности `Deleted`. Это все, что платформе Entity Framework необходимо для удаления сущности.

     Как уже отмечалось, `HttpGet` `Delete` метода не удаляет данные. Выполнение операции delete в ответ на команду GET запроса (или неважно, выполнение любых операций изменения, создайте операции или любой другой операции, изменяющий данные) создает угрозу безопасности. Дополнительные сведения см. в разделе [46 совет # ASP.NET MVC — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Стивен Вальтер блога.
3. В *Views\Student\Delete.cshtml*, добавить сообщение об ошибке между `h2` заголовок и `h3` заголовок, как показано в следующем примере:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Запустите страницу, выбрав **учащихся** и нажав кнопку **удалить** гиперссылки:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Щелкните **Delete** (Удалить). Отображается страница Index (Указатель), на которой удаленный учащийся будет отсутствовать. (Вы увидите пример код обработки ошибок в действие в [обработки параллелизма](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника далее в этой серии.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Проверка подключения к базе данных не остаются открытыми

Чтобы убедиться, что правильно закрыты подключения к базе данных и ресурсы, которые они содержат освобожденные вверх, вы увидите к нему что экземпляр контекста будет удален. Т. е. Почему формирования шаблонов код предоставляет [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) метод в конце `StudentController` класса в *StudentController.cs*, как показано в следующем примере:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Базовый `Controller` уже класс реализует `IDisposable` интерфейс, поэтому этот код просто добавляет переопределение, чтобы `Dispose(bool)` метод при явном удалении экземпляра контекста.

## <a name="summary"></a>Сводка

Теперь у вас есть полный набор страниц, которые выполняют простые операции CRUD для `Student` сущностей. Вспомогательные методы MVC используется для создания элементов пользовательского интерфейса для полей данных. Дополнительные сведения о вспомогательных методов MVC см. в разделе [визуализации с помощью формы HTML Helpers](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (страницы является для MVC 3 но все еще актуальны для MVC 4).

В следующем уроке будет расширяют функциональные возможности страницы индекса, добавив сортировку и разбиение по страницам.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
