---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Реализация базовой функциональности CRUD с Entity Framework в приложении ASP.NET MVC (2 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f580ba6d0db07430e991fba2d061bb2632c95353
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817087"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Реализация базовой функциональности CRUD с Entity Framework в приложении ASP.NET MVC (2 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем руководстве вы создали приложение MVC, которое хранит и отображает данные с помощью Entity Framework и SQL Server LocalDB. В этом руководстве вы сможете просмотреть и настроить CRUD (Создание, чтение, обновление и удаление) кода, формирование шаблонов MVC автоматически создает для вас в контроллерах и представлениях.

> [!NOTE]
> Широко распространена практика реализации шаблона репозитория, позволяющего создать уровень абстракции между контроллером и уровнем доступа к данным. Чтобы упростить эти учебники, не реализуются репозиторием до позднее в учебнике этой серии.


В этом руководстве вы создадите на следующих сайтах:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Создание веб-страницу

Сформированный код для учащихся `Index` исключили страницы `Enrollments` свойство, поскольку оно содержит коллекцию. В `Details` страницы, как отображать содержимое коллекции в таблице HTML.

 В *Controllers\StudentController.cs*, метод действия для `Details` режиме `Find` метод для извлечения одной `Student` сущности. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Значение ключа передается методу в качестве `id` параметр и поступает из данных маршрута в **сведения** гиперссылки на странице индекса. 

1. Откройте *Views\Student\Details.cshtml*. Каждое поле отображается с использованием `DisplayFor` вспомогательного приложения, как показано в следующем примере: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. После `EnrollmentDate` и непосредственно перед закрывающим `fieldset` , добавьте код, чтобы отобразить список регистраций, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Этот код циклически обрабатывает сущности в свойстве навигации `Enrollments`. Для каждого `Enrollment` сущности в свойстве, он отображает название курса и его оценку. Название курса извлекается из `Course` сущность, хранящаяся в `Course` свойство навигации `Enrollments` сущности. Все эти данные извлекаются из базы данных автоматически при необходимости. (Другими словами, при использовании отложенной загрузки здесь. Вы не указали *Безотложная загрузка* для `Courses` свойство навигации, чтобы в первый раз, при попытке доступа к этому свойству, отправляется запрос к базе данных для получения данных. Дополнительные сведения об отложенной загрузки и Безотложная загрузка в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии руководств.)
3. Откройте страницу, выбрав **учащихся** вкладка и нажав кнопку **сведения** ссылку для Александр Carson. Откроется список курсов и оценок для выбранного учащегося:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Обновление страницы создания

1. В *Controllers\StudentController.cs*, замените `HttpPost``Create` метод действия следующий код, чтобы добавить `try-catch` блока и [привязки атрибута](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) в шаблонный метод: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Этот код добавляет `Student` сущность, созданную связывателем модели ASP.NET MVC для `Students` сущности значение и затем сохраняет изменения в базу данных. (*Связывателя модели* относится к функциональным возможностям ASP.NET MVC, который делает он облегчает работу с данными в форме; связыватель модели преобразует учтена формы значения с типами CLR и передает их параметров в метод действия. In this Case, связыватель модели создает `Student` сущности, используя свойство значения из `Form` коллекции.)

    `ValidateAntiForgeryToken` Атрибут позволяет запретить [подделки запросов между сайтами](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак.

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
2. Откройте страницу, выбрав **учащихся** вкладка и щелкнув **Create New**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    По умолчанию работает определенные проверки данных. Введите имена и недопустимую дату и нажмите кнопку **создать** Чтобы просмотреть сообщение об ошибке.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Следующий выделенный код показывает проверку модели.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Измените дату на допустимое значение, например 9/1/2005 и нажмите кнопку **создать** для просмотра нового учащегося, которые отображаются в **индекс** страницы.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Обновление страницы редактирования POST

В *Controllers\StudentController.cs*, `HttpGet` `Edit` метода (без `HttpPost` атрибут) использует `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` метод. Изменять этот метод не нужно.

Тем не менее, замените `HttpPost` `Edit` метод действия следующий код, чтобы добавить `try-catch` блока и [привязки атрибута](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Этот код аналогичен вы видели в `HttpPost` `Create` метод. Тем не менее вместо добавления сущность, созданную связывателем модели к набору сущностей, этот код задает флаг для сущности, указывающий, что он был изменен. Когда [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) вызывается метод, [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) флаг побуждает Entity Framework создает инструкции SQL для обновления строки базы данных. Все столбцы в строке базы данных будут обновлены, включая те, которые пользователь не изменил, и конфликты параллелизма игнорируются. (Вы узнаете, как обеспечивать параллелизм в этом руководстве этой серии.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Состояния сущностей и Attach и методы SaveChanges

Контекст базы данных отслеживает состояние синхронизации сущностей в памяти с соответствующими им строками в базе данных. Данные отслеживания определяют, что происходит при вызове метода `SaveChanges`. Например, при передаче новой сущности для [добавить](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) метод, который присваивается состояние сущности `Added`. При последующем вызове [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метод, контекст базы данных выполняет SQL `INSERT` команды.

Сущность может находиться в одном из[следующие состояния](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Сущность еще не существует в базе данных. `SaveChanges` Метод должен выдать `INSERT` инструкции.
- `Unchanged`. С этой сущностью не нужно выполнять никакие действия с помощью метода `SaveChanges`. Это начальный статус сущности, который она имеет при чтении из базы данных.
- `Modified`. Были изменены значения некоторых или всех свойств сущности. `SaveChanges` Метод должен выдать `UPDATE` инструкции.
- `Deleted`. Сущность отмечена для удаления. `SaveChanges` Метод должен выдать `DELETE` инструкции.
- `Detached`. Сущность не отслеживается контекстом базы данных.

В классическом приложении изменения состояния обычно осуществляются автоматически. В типе приложения рабочего стола считать сущность и вносить изменения в некоторые значения его свойств. В этом случае состояние сущности автоматически изменится на `Modified`. При последующем вызове `SaveChanges`, Entity Framework создает SQL `UPDATE` инструкцию, которая обновляет только конкретный набор свойств, которые были изменены.

Отключены от веб-приложений не допускает это непрерывная последовательность. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , считывающий сущность, ликвидируется после отрисовки страницы. При `HttpPost` `Edit` вызывается метод действия, выполняется новый запрос, и у вас есть новый экземпляр класса [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), поэтому необходимо вручную установить состояние сущности `Modified.` , а затем при вызове `SaveChanges`, Платформа Entity Framework обновляет все столбцы в строке базы данных, поскольку у контекста нет способа узнать, какие свойства были изменены.

Если вы хотите, чтобы SQL `Update` инструкцию, чтобы обновить только поля, которые пользователь фактически изменяет, можно сохранить исходные значения каким-либо образом (например, скрытые поля), чтобы они были доступны при `HttpPost` `Edit` вызывается метод. Затем вы можете создать `Student` сущности, используя исходные значения, вызов `Attach` метод с исходной версией сущности, обновить значения сущности с новыми значениями, а затем вызвать `SaveChanges.` Дополнительные сведения см. в разделе [ Состояния сущностей и SaveChanges](https://msdn.microsoft.com/data/jj592676) и [локальные данные](https://msdn.microsoft.com/data/jj592872) в центре разработчиков MSDN данных.

Код в *Views\Student\Edit.cshtml* аналогичны показанным в *Create.cshtml*, и изменения не требуются.

Откройте страницу, выбрав **учащихся** вкладке и выбрав **изменить** гиперссылки.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Измените определенные данные и нажмите кнопку **Save** (Сохранить). Вы видите измененные данные страницы индекса.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы "Delete"

В *Controllers\StudentController.cs*, код шаблона для `HttpGet` `Delete` использует метод `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` и `Edit` методы. Тем не менее, чтобы реализовать настраиваемое сообщение об ошибке при сбое вызова метода `SaveChanges`, необходимо добавить некоторые функции в этот метод и соответствующее ему представление.

Как и в случае с операциями обновления и создания, операции удаления требуют двух методов действия. Метод, который вызывается в ответ на запрос GET отображает представление, которое дает пользователю возможность подтвердить или отменить операцию удаления. Если пользователь подтверждает ее, создается запрос POST. Когда это происходит, `HttpPost` `Delete` вызывается метод и затем этот метод фактически выполняет операцию удаления.

Вы добавите `try-catch` блок `HttpPost` `Delete` метод для обработки всех ошибок, которые могут возникнуть при обновлении базы данных. Если возникает ошибка, `HttpPost` `Delete` вызовы методов `HttpGet` `Delete` метод, передав ему параметр, указывающий, что произошла ошибка. `HttpGet Delete` Метод повторно отображает страницу подтверждения и сообщение об ошибке, предоставляя пользователю возможность отменить или повторить попытку.

1. Замените `HttpGet` `Delete` метод действия следующий код, который управляет отчеты об ошибках: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Этот код принимает [необязательно](https://msdn.microsoft.com/library/dd264739.aspx) логический параметр, указывающий, был ли он вызван после сбоя, чтобы сохранить изменения. Этот параметр является `false` при `HttpGet` `Delete` метод вызывается без сбоя. Если он вызывается `HttpPost` `Delete` в ответ на ошибки обновления базы данных, параметр является `true` и в представление передается сообщение об ошибке.
2. Замените `HttpPost` `Delete` метода действия (с именем `DeleteConfirmed`) следующим кодом, который выполняет фактическая операция удаления и перехватывает все ошибки обновления базы данных.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Этот код извлекает выбранную сущность и затем вызывает [удалить](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) метод, чтобы присвоить сущности состояние `Deleted`. Когда `SaveChanges` называется SQL `DELETE` команда создается. Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Шаблонный код с именем `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` уникальную сигнатуру метода. (Среда CLR требуется перегруженные методы имели разные параметры метода). Теперь, когда сигнатуры являются уникальными, можно придерживаться соглашения MVC и использовать то же имя для `HttpPost` и `HttpGet` удалить методы.

     Если приоритетом является повышение производительности в приложении большого объема, чтобы избежать создания ненужных запросов SQL для извлечения строки, заменив строки кода, которые вызывают `Find` и `Remove` методы следующим кодом, как показано желтым цветом выделение:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Этот код создает экземпляр `Student` сущности с помощью только значение первичного ключа и затем задает состояние сущности `Deleted`. Это все, что платформе Entity Framework необходимо для удаления сущности.

     Как уже отмечалось, `HttpGet` `Delete` метод данные не удаляются. Выполняет операцию удаления в ответ на запрос GET запроса (или на то пошло, выполнение любых других операций редактирования, создания или другие операции, изменяющей данные) создает угрозу безопасности. Дополнительные сведения см. в разделе [46 совет # ASP.NET MVC — не использовать ссылки, удалить, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) в блоге Стивен Вальтер.
3. В *Views\Student\Delete.cshtml*, добавить сообщение об ошибке между `h2` заголовок и `h3` заголовок, как показано в следующем примере:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Откройте страницу, выбрав **учащихся** вкладка и щелкнув **удалить** гиперссылки:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Щелкните **Delete** (Удалить). Отображается страница Index (Указатель), на которой удаленный учащийся будет отсутствовать. (Вы увидите пример код обработки ошибок в действие в [обработки параллелизма](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии руководств.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Обеспечение подключения к базе данных не остаются открытыми

Чтобы убедиться, что правильно закрыты подключений к базе данных и ресурсы, которые они содержат освобожденные вверх, вы должны увидеть к нему что экземпляр контекста будет удален. То есть, благодаря чему обеспечивает шаблонный код [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) метод в конце `StudentController` в класс *StudentController.cs*, как показано в следующем примере:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Базовый `Controller` реализует класс уже `IDisposable` интерфейс, поэтому этот код просто добавляет переопределение, чтобы `Dispose(bool)` метод явно удалять экземпляр контекста.

## <a name="summary"></a>Сводка

Теперь у вас есть полный набор страниц, которые выполняют простые операции CRUD для `Student` сущностей. Вспомогательные функции MVC используется для создания элементов пользовательского интерфейса для полей данных. Дополнительные сведения о вспомогательных методов MVC, см. в разделе [отрисовка формы с помощью вспомогательных методов HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (страницы является для MVC 3 но он все еще актуальны для MVC 4).

В следующем учебном курсе мы добавим функциональные возможности страницы индекса, добавив сортировку и разбиение по страницам.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
