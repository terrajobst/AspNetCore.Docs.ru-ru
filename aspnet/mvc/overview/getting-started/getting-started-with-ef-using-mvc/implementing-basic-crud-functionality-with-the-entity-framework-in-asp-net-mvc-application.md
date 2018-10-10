---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Реализация базовой функциональности CRUD с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: riande
ms.date: 10/05/2015
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 08b5d38b38d3323e347f0f849ccc0c25fe49efb9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912674"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Реализация базовой функциональности CRUD с Entity Framework в приложении ASP.NET MVC
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

В предыдущем учебнике было создано приложение MVC, которое сохраняет и отображает данные, используя платформу Entity Framework и SQL Server LocalDB. В этом руководстве вы будете просмотрите и настройки создания, читать, обновлять, удаление (CRUD) кода, формирование шаблонов MVC автоматически создает для вас в контроллерах и представлениях.

> [!NOTE]
> Широко распространена практика реализации шаблона репозитория, позволяющего создать уровень абстракции между контроллером и уровнем доступа к данным. Чтобы максимально упростить эти учебники и сконцентрироваться на работе с самой платформой Entity Framework, мы не используем в них репозитории. Сведения о том, как реализовать репозиториев см. в разделе [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

В этом руководстве вы создадите на следующих сайтах:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Создание веб-страницу

Сформированный код для учащихся `Index` исключили страницы `Enrollments` свойство, поскольку оно содержит коллекцию. В `Details` страницы, как отображать содержимое коллекции в таблице HTML.

В *Controllers\StudentController.cs*, метод действия для `Details` режиме [найти](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) метод для извлечения одной `Student` сущности.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Значение ключа передается методу в качестве `id` параметр и поступают из *Маршрутизация данных* в **сведения** гиперссылки на странице индекса.

### <a name="tip-route-data"></a>Совет: **Маршрутизация данных**

Данные маршрута — это данные, которые обнаруживаются связывателем модели в сегмент URL-адреса, указанные в таблице маршрутизации. Например, задает маршрут по умолчанию `controller`, `action`, и `id` сегментов:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

В следующий URL-адрес соответствует маршрут по умолчанию `Instructor` как `controller`, `Index` как `action` и 1 в качестве `id`; это значения данных маршрута.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` значение строки запроса. Связыватель модели также будет работать, если передать `id` значения строки запроса:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

URL-адреса создаются с `ActionLink` инструкций в представлении Razor. В следующем коде `id` параметр соответствует маршруту по умолчанию, поэтому `id` добавляется в качестве данных маршрута.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

В следующем коде `courseID` не соответствует параметру в маршруте по умолчанию, поэтому он добавляется как строку запроса.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Чтобы создать страницу сведений

1. Откройте *Views\Student\Details.cshtml*.

   Каждое поле отображается с использованием `DisplayFor` вспомогательного приложения, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. После `EnrollmentDate` и непосредственно перед закрывающим `</dl>` , добавьте выделенный код, чтобы отобразить список регистраций, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Если кода нарушаются отступы после вставки кода, нажмите клавишу **Ctrl**+**K**, **Ctrl**+**D** для форматирования его.

    Этот код циклически обрабатывает сущности в свойстве навигации `Enrollments`. Для каждого `Enrollment` сущности в свойстве, он отображает название курса и его оценку. Название курса извлекается из `Course` сущность, хранящаяся в `Course` свойство навигации `Enrollments` сущности. Все эти данные извлекаются из базы данных автоматически при необходимости. Другими словами при использовании отложенной загрузки здесь. Вы не указали *Безотложная загрузка* для `Courses` свойство навигации, поэтому регистраций не были получены в тот же запрос, получивший учащихся. Вместо этого при первом обращении к `Enrollments` свойство навигации, новый запрос отправляется в базу данных для получения данных. Дополнительные сведения об отложенной загрузки и Безотложная загрузка в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии руководств.

3. Откройте страницу сведений, запустив программу (**Ctrl**+**F5**), то выбор **учащихся** вкладку, а затем щелкнув **сведения** ссылку для Александр Carson. (Если нажать клавишу **Ctrl**+**F5** хотя *Details.cshtml* файл открыт, появится сообщение об ошибке HTTP 400. Это обусловлено тем, пытается выполнить на странице сведений о Visual Studio, но она не была достигнута по ссылке, указывающим учащегося для отображения. Если это происходит, удалить «Student/подробности» из URL-адрес и повторите попытку, или, закройте браузер, щелкните правой кнопкой мыши проект и нажмите кнопку **представление** > **просмотреть в браузере**.)

    Откроется список курсов и оценок для выбранного учащегося:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

4. Закройте браузер.

## <a name="update-the-create-page"></a>Обновление страницы Create

1. В *Controllers\StudentController.cs*, замените <xref:System.Web.Mvc.HttpPostAttribute> `Create` метод действия, используя следующий код. Этот код добавляет `try-catch` блока и удаляет `ID` из <xref:System.Web.Mvc.BindAttribute> атрибут для шаблонный метод:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Этот код добавляет `Student` сущность, созданную связывателем модели ASP.NET MVC для `Students` сущности значение и затем сохраняет изменения в базу данных. *Связыватель модели* относится к функциональным возможностям ASP.NET MVC, который делает он облегчает работу с данными в форме; связыватель модели преобразует учтена формы значения с типами CLR и передает их параметров в метод действия. В этом случае связыватель модели создает `Student` сущности, используя свойство значения из `Form` коллекции.

    Вы удалены `ID` из привязки атрибут, поскольку `ID` является значение первичного ключа, который SQL Server будет автоматически устанавливаться при вставке строки. Входные данные пользователя не задано `ID` значение.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Предупреждение безопасности — `ValidateAntiForgeryToken` атрибут позволяет запретить [подделки запросов между сайтами](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак. Он требует соответствующего `Html.AntiForgeryToken()` инструкции в представлении, которое станет видно дальше.

    `Bind` Атрибута является одним из способов защиты от *чрезмерной передачи данных* в сценариях создания. Например, предположим, что `Student` сущность включает `Secret` свойство, которое вы не хотите эту веб-страницу для задания.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Даже если у вас нет `Secret` поля на веб-странице, злоумышленник может использовать это средство, например [fiddler](http://fiddler2.com/home), или собственный код JavaScript, для учета `Secret` значения формы. Без <xref:System.Web.Mvc.BindAttribute> ограничивающий поля, которые связыватель модели использует при создании атрибута `Student` экземпляр<em>,</em> связыватель модели выберет это `Secret` значения формы и использовать его для создания `Student` экземпляр сущности. Таким образом, какое бы значение ни задал злоумышленник для поля `Secret`, оно будет обновлено в базе данных. На следующем рисунке показана fiddler средство добавления `Secret` поле (со значением «OverPost») для значения из отправленной формы.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    После этого значение "OverPost" будет успешно добавлено в свойство `Secret` вставленной строки, хотя вы не разрешали установку этого свойства на веб-странице.

    Лучше всего использовать `Include` параметр с `Bind` атрибут *белого списка* поля. Можно также использовать `Exclude` параметр *blacklist* поля, которые требуется исключить. Причина `Include` является более безопасным является, что при добавлении нового свойства в сущность новое поле не защищается автоматически `Exclude` списка.

    Вы можете предотвратить чрезмерную передачу данных в сценариях редактирования — сначала считать сущность из базы данных и последующего вызова `TryUpdateModel`, передавая список явно разрешенных свойств. Это метод, используемый в этих учебниках.

    Альтернативный способ предотвратить чрезмерную передачу данных, который является предпочтительным, многие разработчики — использовать модели представлений вместо классов сущностей с помощью привязки модели. Включайте только те свойства, которые требуется обновлять в модели представления. После завершения работы связывателя модели MVC скопируйте свойства модели представления экземпляр сущности, при необходимости с помощью средства, например [AutoMapper](http://automapper.org/). Используйте db. Запись в экземпляре сущности, чтобы задать его состояние на неизмененное, а затем установите Property("PropertyName"). IsModified в значение true, если каждое свойство сущности, включенный в модели представления. Этот метод подходит для сценариев редактирования и создания.

    Отличное от `Bind` атрибут, `try-catch` блок — это единственное изменение, внесенные в код шаблона. Если во время сохранения изменений перехватывается исключение, производное от <xref:System.Data.DataException>, отображается сообщение об общей ошибке. Исключения <xref:System.Data.DataException> иногда связаны с внешними факторами, а не с ошибкой при программировании приложения, поэтому рекомендуется попробовать повторить выполненные действия снова. В этом примере такое поведение не реализовано, однако в рабочем приложении, как правило, исключения заносятся в журнал. Дополнительные сведения см. в разделе **Ведение журналов для анализа** статьи [Мониторинг и телеметрия (построение реальных облачных приложений для Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Код в *Views\Student\Create.cshtml* аналогичны показанным в *Details.cshtml*, за исключением того, что `EditorFor` и `ValidationMessageFor` вспомогательные функции используются для каждого поля, а не `DisplayFor`. Вот соответствующий код:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *CREATE.chstml* также включает в себя `@Html.AntiForgeryToken()`, который работает с `ValidateAntiForgeryToken` атрибута в контроллере, чтобы предотвратить [подделки запросов между сайтами](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак.

    Изменения не требуются в *Create.cshtml*.

2. Запустить эту страницу, запустив программу, выбрав **учащихся** вкладку, а затем щелкнув **Create New**.

3. Введите имена и недопустимую дату и нажмите кнопку **создать** Чтобы просмотреть сообщение об ошибке.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Это проверка на стороне сервера, вы получаете по умолчанию. В этом руководстве вы увидите, как добавлять атрибуты, которые создают код для проверки на стороне клиента. Следующий выделенный код показывает проверку модели в **создать** метод.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Измените дату на допустимую и щелкните **Create** (Создать), чтобы добавить нового учащегося на страницу **Index** (Указатель).

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

5. Закройте браузер.

## <a name="update-the-edit-httppost-method"></a>Обновите метод HttpPost, изменение

1. Замените <xref:System.Web.Mvc.HttpPostAttribute> `Edit` метод действия, используя следующий код:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > В *Controllers\StudentController.cs*, `HttpGet Edit` метода (без `HttpPost` атрибут) использует `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details`метод. Изменять этот метод не нужно.

   Благодаря этому изменению реализуются рекомендации безопасности, чтобы предотвратить [чрезмерную передачу данных](#overpost), шаблон создал `Bind` атрибут и добавил сущность, созданную связывателем модели к набору с флагом измененных сущностей. Что код больше не рекомендуется, так как `Bind` атрибут очищает любые ранее существовавшие данные в поля, не указанные в `Include` параметра. В будущем, шаблон MVC контроллер обновляется таким образом, чтобы он не создавал `Bind` атрибуты для методов Edit.

   Новый код считывает существующую сущность и вызывает <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> для обновления полей из введенных пользователем данных в отправленной формы данных. Автоматическое отслеживание наборов изменений платформы Entity Framework [EntityState.Modified](<xref:System.Data.EntityState.Modified>) флаг для сущности. Когда [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) вызывается метод, <xref:System.Data.EntityState.Modified> флаг побуждает Entity Framework создает инструкции SQL для обновления строки базы данных. [Конфликты параллелизма](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) игнорируются, и обновляются все столбцы в строке базы данных, включая те, которые пользователь не изменил. (Позднее в учебнике показано, как обрабатывать конфликты параллелизма, и если вам требуется только отдельные поля для обновления в базе данных, можно разместить сущности [EntityState.Unchanged](<xref:System.Data.EntityState.Unchanged>) и задать отдельные поля [ EntityState.Modified](<xref:System.Data.EntityState.Modified>).)

   Чтобы предотвратить чрезмерную передачу данных, поля, которые требуется обновлять страницы "Edit" будут добавлены в список разрешений в `TryUpdateModel` параметров. На данный момент другие поля не защищаются. Если включить в список поля, которые должен привязывать связыватель модели, это позволяет гарантировать, что при добавлении полей в модель данных в будущем они будут автоматически защищаться до тех пор, пока вы явно не добавите их сюда.

   В результате этих изменений сигнатура метода метода HttpPost Edit совпадает со значением метода HttpGet edit; Таким образом, вы просто переименовали метод EditPost.

   > [!TIP]
   >
   > **Состояния сущностей и Attach и методы SaveChanges**
   >
   > Контекст базы данных отслеживает состояние синхронизации сущностей в памяти с соответствующими им строками в базе данных. Данные отслеживания определяют, что происходит при вызове метода `SaveChanges`. Например, при передаче новой сущности для [добавить](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) метод, который присваивается состояние сущности `Added`. При последующем вызове [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метод, контекст базы данных выполняет SQL `INSERT` команды.
   >
   > Сущность может находиться в одном из следующих [состояний](xref:System.Data.EntityState):
   >
   > - `Added`. Сущность еще не существует в базе данных. `SaveChanges` Метод должен выдать `INSERT` инструкции.
   > - `Unchanged`. С этой сущностью не нужно выполнять никакие действия с помощью метода `SaveChanges`. Это начальный статус сущности, который она имеет при чтении из базы данных.
   > - `Modified`. Были изменены значения некоторых или всех свойств сущности. `SaveChanges` Метод должен выдать `UPDATE` инструкции.
   > - `Deleted`. Сущность отмечена для удаления. `SaveChanges` Метод должен выдать `DELETE` инструкции.
   > - `Detached`. Сущность не отслеживается контекстом базы данных.
   >
   > В классическом приложении изменения состояния обычно осуществляются автоматически. В типе приложения рабочего стола считать сущность и вносить изменения в некоторые значения его свойств. В этом случае состояние сущности автоматически изменится на `Modified`. При последующем вызове `SaveChanges`, Entity Framework создает SQL `UPDATE` инструкцию, которая обновляет только конкретный набор свойств, которые были изменены.
   >
   > Отключены от веб-приложений не допускает это непрерывная последовательность. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , считывающий сущность, ликвидируется после отрисовки страницы. При `HttpPost` `Edit` вызывается метод действия, выполняется новый запрос, и у вас есть новый экземпляр класса [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), поэтому необходимо вручную установить состояние сущности `Modified.` , а затем при вызове `SaveChanges`, Платформа Entity Framework обновляет все столбцы в строке базы данных, поскольку у контекста нет способа узнать, какие свойства были изменены.
   >
   > Если вы хотите, чтобы SQL `Update` инструкцию, чтобы обновить только поля, которые пользователь фактически изменяет, можно сохранить исходные значения каким-либо образом (например, скрытые поля), чтобы они были доступны при `HttpPost` `Edit` вызывается метод. Затем вы можете создать `Student` сущности, используя исходные значения, вызов `Attach` метод с исходной версией сущности, обновить значения сущности с новыми значениями, а затем вызвать `SaveChanges.` Дополнительные сведения см. в разделе [ Состояния сущностей и SaveChanges](/ef/ef6/saving/change-tracking/entity-state) и [локальных данных](/ef/ef6/querying/local-data).

   Код HTML и Razor в *Views\Student\Edit.cshtml* аналогичны показанным в *Create.cshtml*, и изменения не требуются.

2. Запустить эту страницу, запустив программу, выбрав **учащихся** вкладку, а затем щелкнув **изменить** гиперссылки.

   ![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

3. Измените определенные данные и нажмите кнопку **Save** (Сохранить). Вы видите измененные данные страницы индекса.

   ![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

4. Закройте браузер.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

В *Controllers\StudentController.cs*, код шаблона для <xref:System.Web.Mvc.HttpGetAttribute> `Delete` использует метод `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` и `Edit` методы. Тем не менее, чтобы реализовать настраиваемое сообщение об ошибке при сбое вызова метода `SaveChanges`, необходимо добавить некоторые функции в этот метод и соответствующее ему представление.

Как и в случае с операциями обновления и создания, операции удаления требуют двух методов действия. Метод, который вызывается в ответ на запрос GET отображает представление, которое дает пользователю возможность подтвердить или отменить операцию удаления. Если пользователь подтверждает ее, создается запрос POST. Когда это происходит, `HttpPost` `Delete` вызывается метод и затем этот метод фактически выполняет операцию удаления.

Вы добавите `try-catch` блок <xref:System.Web.Mvc.HttpPostAttribute> `Delete` метод для обработки всех ошибок, которые могут возникнуть при обновлении базы данных. Если возникает ошибка, <xref:System.Web.Mvc.HttpPostAttribute> `Delete` вызовы методов <xref:System.Web.Mvc.HttpGetAttribute> `Delete` метод, передав ему параметр, указывающий, что произошла ошибка. <xref:System.Web.Mvc.HttpGetAttribute> `Delete` Метод повторно отображает страницу подтверждения и сообщение об ошибке, предоставляя пользователю возможность отменить или повторить попытку.

1. Замените <xref:System.Web.Mvc.HttpGetAttribute> `Delete` метод действия следующий код, который управляет отчеты об ошибках:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Этот код принимает [необязательный параметр](https://msdn.microsoft.com/library/dd264739.aspx) , указывающее, был ли метод вызван после сбоя, чтобы сохранить изменения. Этот параметр является `false` при `HttpGet` `Delete` метод вызывается без сбоя. Если он вызывается `HttpPost` `Delete` в ответ на ошибки обновления базы данных, параметр является `true` и в представление передается сообщение об ошибке.

2. Замените <xref:System.Web.Mvc.HttpPostAttribute> `Delete` метода действия (с именем `DeleteConfirmed`) следующим кодом, который выполняет фактическая операция удаления и перехватывает все ошибки обновления базы данных.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Этот код извлекает выбранную сущность и затем вызывает [удалить](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) метод, чтобы присвоить сущности состояние `Deleted`. Когда `SaveChanges` называется SQL `DELETE` команда создается. Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Шаблонный код с именем `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` уникальную сигнатуру метода. (Среда CLR требует, чтобы перегруженные методы имели разные параметры метода.) Теперь, когда сигнатуры являются уникальными, можно придерживаться соглашения MVC и использовать то же имя для `HttpPost` и `HttpGet` удалить методы.

    Если приоритетом является повышение производительности в приложении большого объема, чтобы избежать создания ненужных запросов SQL для извлечения строки, заменив строки кода, которые вызывают `Find` и `Remove` методы следующим кодом:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Этот код создает экземпляр `Student` сущности с помощью только значение первичного ключа и затем задает состояние сущности `Deleted`. Это все, что платформе Entity Framework необходимо для удаления сущности.

    Как уже отмечалось, `HttpGet` `Delete` метод данные не удаляются. Выполняет операцию удаления в ответ на запрос GET запроса (или на то пошло, выполнение любых других операций редактирования, создания или другие операции, изменяющей данные) создает угрозу безопасности. Дополнительные сведения см. в разделе [46 совет # ASP.NET MVC — не использовать ссылки, удалить, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) в блоге Стивен Вальтер.

3. В *Views\Student\Delete.cshtml*, добавить сообщение об ошибке между `h2` заголовок и `h3` заголовок, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Запустить эту страницу, запустив программу, выбрав **учащихся** вкладку, а затем щелкнув **удалить** гиперссылки:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

5. Выберите **удалить** на странице, — говорит **Вы действительно хотите удалить это?**.

    Страница индекса отображает которой удаленный учащийся. (Вы увидите пример код обработки ошибок в действие в [руководстве параллелизма](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Подключения к базе данных закрыть

Чтобы закрыть подключения к базе данных и освободить ресурсы, которые они содержат как можно скорее, dispose экземпляр контекста, когда вы закончите с ним. То есть, благодаря чему обеспечивает шаблонный код [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) метод в конце `StudentController` в класс *StudentController.cs*, как показано в следующем примере:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Базовый `Controller` реализует класс уже `IDisposable` интерфейс, поэтому этот код просто добавляет переопределение, чтобы `Dispose(bool)` метод явно удалять экземпляр контекста.

## <a name="handle-transactions"></a>Обработка транзакций

По умолчанию платформа Entity Framework реализует транзакции неявно. В сценариях, где вы вносите изменения в несколько строк или таблиц и затем вызвать `SaveChanges`, Entity Framework автоматически гарантирует, что все изменения завершится успехом или отказать. Если ошибка происходит после того, как были выполнены некоторые изменения, эти изменения автоматически откатываются. Для сценариев, где требуется дополнительный контроль&mdash;к примеру, если требуется включить операции, выполняемые вне платформы Entity Framework в транзакции&mdash;см. в разделе [работа с транзакциями](/ef/ef6/saving/transactions).

## <a name="summary"></a>Сводка

Теперь у вас есть полный набор страниц, которые выполняют простые операции CRUD для `Student` сущностей. Вспомогательные функции MVC используется для создания элементов пользовательского интерфейса для полей данных. Дополнительные сведения о вспомогательных методов MVC, см. в разделе [отрисовка формы с помощью вспомогательных методов HTML](/previous-versions/aspnet/dd410596(v=vs.98)) (статья предназначена для MVC 3 но он все еще актуальны для MVC 5).

В следующем учебном курсе будет расширяют функциональные возможности страницы индекса, добавив сортировку и разбиение по страницам.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить.

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
