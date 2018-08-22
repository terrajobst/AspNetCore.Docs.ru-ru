---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Реализация базовой функциональности CRUD с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: riande
ms.date: 03/09/2015
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd718cfe0f257bbf97f5ef3fdbdedecc88fc4e98
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837610"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Реализация базовой функциональности CRUD с Entity Framework в приложении ASP.NET MVC
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем руководстве вы создали приложение MVC, которое хранит и отображает данные с помощью Entity Framework и SQL Server LocalDB. В этом руководстве вы сможете просмотреть и настроить CRUD (Создание, чтение, обновление и удаление) кода, формирование шаблонов MVC автоматически создает для вас в контроллерах и представлениях.

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

«? courseID = 2021» — это значения строки запроса. Связыватель модели также будет работать, если передать `id` значения строки запроса:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

URL-адреса создаются с `ActionLink` инструкций в представлении Razor. В следующем коде `id` параметр соответствует маршруту по умолчанию, поэтому `id` добавляется в качестве данных маршрута.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

В следующем коде `courseID` не соответствует параметру в маршруте по умолчанию, поэтому он добавляется как строку запроса.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Откройте *Views\Student\Details.cshtml*. Каждое поле отображается с использованием `DisplayFor` вспомогательного приложения, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. После `EnrollmentDate` и непосредственно перед закрывающим `</dl>` , добавьте выделенный код, чтобы отобразить список регистраций, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Если после вставки кода нарушаются отступы в нем, нажмите клавиши CTRL-K-D, чтобы исправить это.

    Этот код циклически обрабатывает сущности в свойстве навигации `Enrollments`. Для каждого `Enrollment` сущности в свойстве, он отображает название курса и его оценку. Название курса извлекается из `Course` сущность, хранящаяся в `Course` свойство навигации `Enrollments` сущности. Все эти данные извлекаются из базы данных автоматически при необходимости. (Другими словами, при использовании отложенной загрузки здесь. Вы не указали *Безотложная загрузка* для `Courses` свойство навигации, поэтому регистраций не были получены в тот же запрос, получивший учащихся. Вместо этого при первом обращении к `Enrollments` свойство навигации, новый запрос отправляется в базу данных для получения данных. Дополнительные сведения об отложенной загрузки и Безотложная загрузка в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии руководств.)
3. Откройте страницу, выбрав **учащихся** вкладка и нажав кнопку **сведения** ссылку для Александр Carson. (Если нажать клавишу CTRL + F5 при открытом файле Details.cshtml, вы получите ошибку HTTP 400, так как Visual Studio пытается запустить страницу сведений, но она не была достигнута по ссылке, указывающим учащегося для отображения. Изменяйте, просто удалите «Student/подробности» из URL-адрес и повторите попытку, или закройте браузер, щелкните правой кнопкой мыши проект и нажмите кнопку **представление**, а затем нажмите кнопку **просмотреть в браузере**.)

    Откроется список курсов и оценок для выбранного учащегося:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Обновление страницы Create

1. В *Controllers\StudentController.cs*, замените `HttpPost``Create` метод действия следующий код, чтобы добавить `try-catch` заблокировать и удалить `ID` из [привязки атрибута](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) для шаблонный метод:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Этот код добавляет `Student` сущность, созданную связывателем модели ASP.NET MVC для `Students` сущности значение и затем сохраняет изменения в базу данных. (*Связывателя модели* относится к функциональным возможностям ASP.NET MVC, который делает он облегчает работу с данными в форме; связыватель модели преобразует учтена формы значения с типами CLR и передает их параметров в метод действия. In this Case, связыватель модели создает `Student` сущности, используя свойство значения из `Form` коллекции.)

    Вы удалены `ID` из привязки атрибут, поскольку `ID` является значение первичного ключа, который SQL Server будет автоматически устанавливаться при вставке строки. Входные данные пользователя не задано `ID` значение.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Предупреждение безопасности — `ValidateAntiForgeryToken` атрибут позволяет запретить [подделки запросов между сайтами](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак. Он требует соответствующего `Html.AntiForgeryToken()` инструкции в представлении, которое станет видно дальше.

    `Bind` Атрибута является одним из способов защиты от *чрезмерной передачи данных* в сценариях создания. Например, предположим, что `Student` сущность включает `Secret` свойство, которое вы не хотите эту веб-страницу для задания.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Даже если у вас нет `Secret` поля на веб-странице, злоумышленник может использовать это средство, например [fiddler](http://fiddler2.com/home), или собственный код JavaScript, для учета `Secret` значения формы. Без [привязать](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) ограничивающий поля, которые связыватель модели использует при создании атрибута `Student` экземпляр<em>,</em> связыватель модели выберет это `Secret` значения формы и использовать его для Создание `Student` экземпляра сущности. Таким образом, какое бы значение ни задал злоумышленник для поля `Secret`, оно будет обновлено в базе данных. На следующем рисунке показана fiddler средство добавления `Secret` поле (со значением «OverPost») для значения из отправленной формы.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    После этого значение "OverPost" будет успешно добавлено в свойство `Secret` вставленной строки, хотя вы не разрешали установку этого свойства на веб-странице.

    Это безопасности рекомендуется использовать `Include` параметр с `Bind` атрибут *белого списка* поля. Можно также использовать `Exclude` параметр *blacklist* поля, которые требуется исключить. Причина `Include` является более безопасным является, что при добавлении нового свойства в сущность новое поле не защищается автоматически `Exclude` списка.

    Вы можете предотвратить чрезмерную передачу данных в сценариях редактирования — сначала считать сущность из базы данных и последующего вызова `TryUpdateModel`, передавая список явно разрешенных свойств. Это метод, используемый в этих учебниках.

    Альтернативный способ предотвратить чрезмерную передачу данных, который является предпочтительным, многие разработчики — использовать модели представлений вместо классов сущностей с помощью привязки модели. Включайте только те свойства, которые требуется обновлять в модели представления. После завершения работы связывателя модели MVC скопируйте свойства модели представления экземпляр сущности, при необходимости с помощью средства, например [AutoMapper](http://automapper.org/). Используйте db. Запись в экземпляре сущности, чтобы задать его состояние на неизмененное, а затем установите Property("PropertyName"). IsModified в значение true, если каждое свойство сущности, включенный в модели представления. Этот метод подходит для сценариев редактирования и создания.

    Отличное от `Bind` атрибут, `try-catch` блок — это единственное изменение, внесенные в код шаблона. Если исключение, которое является производным от [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) будет перехвачено, пока сохраняются изменения, отображается сообщение об общей ошибке. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) исключения иногда связаны с приложением, а не на наличие программной ошибки, внешними факторами, поэтому пользователю предлагается повторить попытку. В этом примере такое поведение не реализовано, однако в рабочем приложении, как правило, исключения заносятся в журнал. Дополнительные сведения см. в разделе **Ведение журналов для анализа** статьи [Мониторинг и телеметрия (построение реальных облачных приложений для Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Код в *Views\Student\Create.cshtml* аналогичны показанным в *Details.cshtml*, за исключением того, что `EditorFor` и `ValidationMessageFor` вспомогательные функции используются для каждого поля, а не `DisplayFor`. Вот соответствующий код:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *CREATE.chstml* также включает в себя `@Html.AntiForgeryToken()`, который работает с `ValidateAntiForgeryToken` атрибута в контроллере, чтобы предотвратить [подделки запросов между сайтами](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак.

    Изменения не требуются в *Create.cshtml*.
2. Откройте страницу, выбрав **учащихся** вкладка и щелкнув **Create New**.
3. Введите имена и недопустимую дату и нажмите кнопку **создать** Чтобы просмотреть сообщение об ошибке.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Эта проверка по умолчанию выполняется на стороне сервера. Позднее в учебнике вы узнаете, как добавлять атрибуты, которые будут создавать код для проверки на стороне клиента. Следующий выделенный код показывает проверку модели в **создать** метод.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Измените дату на допустимую и щелкните **Create** (Создать), чтобы добавить нового учащегося на страницу **Index** (Указатель).

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Обновление метода HttpPost Edit

В *Controllers\StudentController.cs*, `HttpGet` `Edit` метода (без `HttpPost` атрибут) использует `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` метод. Изменять этот метод не нужно.

Тем не менее, замените `HttpPost` `Edit` метод действия, используя следующий код:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Благодаря этому изменению реализуются рекомендации безопасности, чтобы предотвратить [чрезмерную передачу данных](#overpost), шаблон создал `Bind` атрибут и добавил сущность, созданную связывателем модели к набору с флагом измененных сущностей. Что код больше не рекомендуется, так как `Bind` атрибут очищает любые ранее существовавшие данные в поля, не указанные в `Include` параметра. В будущем, шаблон MVC контроллер обновляется таким образом, чтобы он не создавал `Bind` атрибуты для методов Edit.

Новый код считывает существующую сущность и вызывает [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) для обновления полей из введенных пользователем данных в отправленной формы данных. Автоматическое отслеживание наборов изменений платформы Entity Framework [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) флаг для сущности. Когда [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) вызывается метод, `Modified` флаг побуждает Entity Framework создает инструкции SQL для обновления строки базы данных. [Конфликты параллелизма](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) игнорируются, и обновляются все столбцы в строке базы данных, включая те, которые пользователь не изменил. (Позднее в учебнике показано, как обрабатывать конфликты параллелизма, и если вам требуется только отдельные поля для обновления в базе данных, можно присвоить Unchanged сущности и задать отдельные поля для изменения.)

Рекомендуется, чтобы предотвратить чрезмерную передачу данных, поля, которые требуется обновлять страницы "Edit" будут добавлены в список разрешений в `TryUpdateModel` параметров. На данный момент другие поля не защищаются. Если включить в список поля, которые должен привязывать связыватель модели, это позволяет гарантировать, что при добавлении полей в модель данных в будущем они будут автоматически защищаться до тех пор, пока вы явно не добавите их сюда.

В результате этих изменений сигнатура метода метода HttpPost Edit совпадает со значением метода HttpGet edit; Таким образом, вы просто переименовали метод EditPost.

> [!TIP] 
> 
> **Состояния сущностей и Attach и методы SaveChanges**
> 
> Контекст базы данных отслеживает состояние синхронизации сущностей в памяти с соответствующими им строками в базе данных. Данные отслеживания определяют, что происходит при вызове метода `SaveChanges`. Например, при передаче новой сущности для [добавить](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) метод, который присваивается состояние сущности `Added`. При последующем вызове [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метод, контекст базы данных выполняет SQL `INSERT` команды.
> 
> Сущность может находиться в одном из[следующие состояния](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
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
> Если вы хотите, чтобы SQL `Update` инструкцию, чтобы обновить только поля, которые пользователь фактически изменяет, можно сохранить исходные значения каким-либо образом (например, скрытые поля), чтобы они были доступны при `HttpPost` `Edit` вызывается метод. Затем вы можете создать `Student` сущности, используя исходные значения, вызов `Attach` метод с исходной версией сущности, обновить значения сущности с новыми значениями, а затем вызвать `SaveChanges.` Дополнительные сведения см. в разделе [ Состояния сущностей и SaveChanges](https://msdn.microsoft.com/data/jj592676) и [локальные данные](https://msdn.microsoft.com/data/jj592872) в центре разработчиков MSDN данных.


Код HTML и Razor в *Views\Student\Edit.cshtml* аналогичны показанным в *Create.cshtml*, и изменения не требуются.

Откройте страницу, выбрав **учащихся** вкладке и выбрав **изменить** гиперссылки.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Измените определенные данные и нажмите кнопку **Save** (Сохранить). Вы видите измененные данные страницы индекса.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Обновление страницы "Delete"

В *Controllers\StudentController.cs*, код шаблона для `HttpGet` `Delete` использует метод `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` и `Edit` методы. Тем не менее, чтобы реализовать настраиваемое сообщение об ошибке при сбое вызова метода `SaveChanges`, необходимо добавить некоторые функции в этот метод и соответствующее ему представление.

Как и в случае с операциями обновления и создания, операции удаления требуют двух методов действия. Метод, который вызывается в ответ на запрос GET отображает представление, которое дает пользователю возможность подтвердить или отменить операцию удаления. Если пользователь подтверждает ее, создается запрос POST. Когда это происходит, `HttpPost` `Delete` вызывается метод и затем этот метод фактически выполняет операцию удаления.

Вы добавите `try-catch` блок `HttpPost` `Delete` метод для обработки всех ошибок, которые могут возникнуть при обновлении базы данных. Если возникает ошибка, `HttpPost` `Delete` вызовы методов `HttpGet` `Delete` метод, передав ему параметр, указывающий, что произошла ошибка. `HttpGet Delete` Метод повторно отображает страницу подтверждения и сообщение об ошибке, предоставляя пользователю возможность отменить или повторить попытку.

1. Замените `HttpGet` `Delete` метод действия следующий код, который управляет отчеты об ошибках: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Этот код принимает [необязательный параметр](https://msdn.microsoft.com/library/dd264739.aspx) , указывающее, был ли метод вызван после сбоя, чтобы сохранить изменения. Этот параметр является `false` при `HttpGet` `Delete` метод вызывается без сбоя. Если он вызывается `HttpPost` `Delete` в ответ на ошибки обновления базы данных, параметр является `true` и в представление передается сообщение об ошибке.
2. Замените `HttpPost` `Delete` метода действия (с именем `DeleteConfirmed`) следующим кодом, который выполняет фактическая операция удаления и перехватывает все ошибки обновления базы данных.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     Этот код извлекает выбранную сущность и затем вызывает [удалить](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) метод, чтобы присвоить сущности состояние `Deleted`. Когда `SaveChanges` называется SQL `DELETE` команда создается. Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Шаблонный код с именем `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` уникальную сигнатуру метода. (Среда CLR требуется перегруженные методы имели разные параметры метода). Теперь, когда сигнатуры являются уникальными, можно придерживаться соглашения MVC и использовать то же имя для `HttpPost` и `HttpGet` удалить методы.

     Если приоритетом является повышение производительности в приложении большого объема, чтобы избежать создания ненужных запросов SQL для извлечения строки, заменив строки кода, которые вызывают `Find` и `Remove` методы следующим кодом:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     Этот код создает экземпляр `Student` сущности с помощью только значение первичного ключа и затем задает состояние сущности `Deleted`. Это все, что платформе Entity Framework необходимо для удаления сущности.

     Как уже отмечалось, `HttpGet` `Delete` метод данные не удаляются. Выполняет операцию удаления в ответ на запрос GET запроса (или на то пошло, выполнение любых других операций редактирования, создания или другие операции, изменяющей данные) создает угрозу безопасности. Дополнительные сведения см. в разделе [46 совет # ASP.NET MVC — не использовать ссылки, удалить, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) в блоге Стивен Вальтер.
3. В *Views\Student\Delete.cshtml*, добавить сообщение об ошибке между `h2` заголовок и `h3` заголовок, как показано в следующем примере:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     Откройте страницу, выбрав **учащихся** вкладка и щелкнув **удалить** гиперссылки:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. Щелкните **Delete** (Удалить). Отображается страница Index (Указатель), на которой удаленный учащийся будет отсутствовать. (Вы увидите пример код обработки ошибок в действие в [руководстве параллелизма](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Закрытие подключений к базе данных

Чтобы закрыть подключения к базе данных и освободить ресурсы, которые они содержат как можно скорее, dispose экземпляр контекста, когда вы закончите с ним. То есть, благодаря чему обеспечивает шаблонный код [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) метод в конце `StudentController` в класс *StudentController.cs*, как показано в следующем примере:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Базовый `Controller` реализует класс уже `IDisposable` интерфейс, поэтому этот код просто добавляет переопределение, чтобы `Dispose(bool)` метод явно удалять экземпляр контекста.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Обработка транзакций

По умолчанию платформа Entity Framework реализует транзакции неявно. В сценариях, где вы вносите изменения в несколько строк или таблиц и затем вызвать `SaveChanges`, Entity Framework автоматически гарантирует, что все изменения завершится успехом или отказать. Если ошибка происходит после того, как были выполнены некоторые изменения, эти изменения автоматически откатываются. См. в сценариях, где требуется дополнительный контроль — например, если требуется включить операции, выполняемые вне платформы Entity Framework в транзакции-- [работа с транзакциями](https://msdn.microsoft.com/data/dn456843) на сайте MSDN.

## <a name="summary"></a>Сводка

Теперь у вас есть полный набор страниц, которые выполняют простые операции CRUD для `Student` сущностей. Вспомогательные функции MVC используется для создания элементов пользовательского интерфейса для полей данных. Дополнительные сведения о вспомогательных методов MVC, см. в разделе [отрисовка формы с помощью вспомогательных методов HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (страницы является для MVC 3 но он все еще актуальны для MVC 5).

В следующем учебном курсе мы добавим функциональные возможности страницы индекса, добавив сортировку и разбиение по страницам.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить. Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
