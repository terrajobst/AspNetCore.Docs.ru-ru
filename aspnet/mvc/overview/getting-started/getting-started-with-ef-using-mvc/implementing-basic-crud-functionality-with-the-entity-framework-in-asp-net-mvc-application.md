---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "Реализация функциональности основные CRUD с платформой Entity Framework в приложение ASP.NET MVC | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e3dbea51199722bfe50f201c4ddcc90aa081927d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Реализация функциональности основные CRUD с платформой Entity Framework в приложение ASP.NET MVC
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебнике вы создали приложение MVC, хранит и отображает данные с помощью платформы Entity Framework и SQL Server LocalDB. В этом учебнике предстоит просмотреть и настроить CRUD (Создание, чтение, обновление и удаление) код, который MVC функции формирования шаблонов автоматически создает для вас в контроллеры и представления.

> [!NOTE]
> Он часто используется для реализации шаблона репозитория, чтобы создать уровень абстракции между контроллер и уровня доступа к данным. Чтобы сохранить эти учебники проста и заключается в учит способы использования платформы Entity Framework сам, они не используйте репозиториев. Сведения о реализации репозиториев. в разделе [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).


В этом учебнике вы создадите на следующих сайтах:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Создать веб-страницу

Код формирования шаблонов для учащихся `Index` опущены страницы `Enrollments` свойства, так как это свойство содержит коллекцию. В `Details` страницы, как отображать содержимое коллекции в HTML-таблицу.

 В *Controllers\StudentController.cs*, метод действия для `Details` просмотра использует [найти](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) метод для извлечения одной `Student` сущности. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Значение ключа передается методу в качестве `id` параметр и поступают из *маршрутизации данных* в **сведения** гиперссылки на странице индекса.

### <a name="tip-route-data"></a>Совет: **маршрутизации данных**

Данные маршрута — это данные, связывателя модели найден в сегмент URL-адреса, указанные в таблице маршрутизации. Например, задает маршрут по умолчанию `controller`, `action`, и `id` сегментов:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

В следующий URL-адрес маршрута по умолчанию соответствует `Instructor` как `controller`, `Index` как `action` и 1 в качестве `id`; это значения данных маршрута.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

«? courseID = 2021» имеет значение строки запроса. Связыватель модели также будет работать, если передать `id` как значение строки запроса:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

URL-адреса, созданные `ActionLink` инструкций в представлении Razor. В следующем коде `id` параметр соответствует маршрут по умолчанию, поэтому `id` добавляется в данные маршрута.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

В следующем коде `courseID` не совпадает с параметром в маршрут по умолчанию, он добавляется в виде строки запроса.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Open *Views\Student\Details.cshtml*. Каждое поле отображается с помощью `DisplayFor` вспомогательного приложения, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. После `EnrollmentDate` и непосредственно перед закрывающим тегом `</dl>` , добавьте выделенный код, чтобы отобразить список регистраций, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Если коду отступ неправильный после вставки кода, нажмите клавиши CTRL-K-D, чтобы исправить его.

    Этот код выполняет цикл по сущности в `Enrollments` свойство навигации. Для каждого `Enrollment` сущности в свойстве, оно отображает название курса и уровень. Название курса извлекается из `Course` сущность, которая хранится в `Course` свойство навигации `Enrollments` сущности. Все эти данные извлекаются из базы данных автоматически при необходимости. (Другими словами, вы используете отложенную загрузку, здесь. Вы не указали *упреждающую* для `Courses` свойство навигации, поэтому регистрации не были получены в том же запросе, получившее слушателей. Вместо этого первый раз, при попытке открыть `Enrollments` свойство навигации, новый запрос отправляется в базу данных для получения данных. Вы можете прочитать больше о отложенную загрузку и упреждающую в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника далее в этой серии.)
3. Запустите страницу, выбрав **учащихся** и нажав кнопку **сведения** ссылку для Александр Carson. (При нажатии клавиши CTRL + F5 при открытом файле Details.cshtml, вы получите ошибку HTTP 400, поскольку Visual Studio пытается запустить страницу сведений, но не был достигнут по ссылке, указывает студента для отображения. In that Case, просто удалите «Студента/подробности» из URL-адрес и повторите попытку, или закройте браузер, щелкните правой кнопкой мыши проект и нажмите кнопку **представление**, а затем нажмите кнопку **Просмотр в браузере**.)

    Просмотреть список курсов и оценок для выбранного учащегося:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Обновление страницы создания

1. В *Controllers\StudentController.cs*, замените `HttpPost``Create` метод действия на следующий код, чтобы добавить `try-catch` блокировку и удалить `ID` из [атрибута Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) для метод формирования шаблонов:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Этот код добавляет `Student` сущности, созданные связывателя модели ASP.NET MVC для `Students` набор сущности и затем сохраняет изменения в базе данных. (*Связывателя модели* ссылается на функции ASP.NET MVC, которые делают его проще работать с данными, отправленных в форме; связыватель модели формы учтена преобразует значения с типами среды CLR и передает их в параметры метода действия. In this Case, создает экземпляр связывателя модели `Student` сущности можно с помощью свойства значения из `Form` коллекции.)

    Вы удалены `ID` из привязки атрибута, так как `ID` имеет значение первичного ключа, который SQL Server будет автоматически устанавливать при вставке строки. Входные данные пользователя не задано `ID` значение.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Предупреждение безопасности — `ValidateAntiForgeryToken` атрибут помогает предотвратить [межсайтовой подделки запросов](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак. Требуется соответствующий `Html.AntiForgeryToken()` инструкции в представление, в котором будет показано ниже.

    `Bind` Атрибут является одним из способов защиты от *чрезмерно учета* в создавать сценарии. Например, предположим, что `Student` сущность включает `Secret` свойство, которое необходимо исключить из этой веб-страницы для задания.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Даже при отсутствии `Secret` поле на веб-странице, злоумышленник может использовать это средство, например [fiddler](http://fiddler2.com/home), или написать сценарий JavaScript для учета `Secret` формирования значения. Без [привязки](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) ограничение поля, которые использует связыватель модели при создании атрибута `Student` экземпляр*,* , будет окрашена связывателя модели `Secret` форму значение и использовать его для Создание `Student` экземпляра сущности. Выберите любое другое значение злоумышленнику, указанный для `Secret` поля формы будет обновлено в базе данных. На следующем рисунке показана fiddler средство добавления `Secret` поле (со значением «OverPost») для значения из отправленной формы.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    Значение «OverPost» был бы затем успешно добавлен `Secret` свойство вставленных строк, несмотря на то, что вы никогда не предназначен, что веб-страницы можно задать это свойство.

    Это соображениям безопасности рекомендуется использовать `Include` параметр с `Bind` атрибут *белого списка* поля. Можно также использовать `Exclude` параметр *черный список* поля, которые требуется исключить. Причина `Include` безопаснее, — что при добавлении нового свойства сущности, новое поле не защищен автоматически `Exclude` списка.

    Можно предотвратить overposting в сценариях редактирования — сначала чтение сущности из базы данных и последующего вызова `TryUpdateModel`, передавая список разрешенных явных свойств. Это метод, используемый в этих учебниках.

    Альтернативный способ предотвратить overposting, который является предпочтительным, многие разработчики — использовать Просмотр моделей, а не классы сущностей с использованием привязки модели. Включить только те свойства, необходимые для обновления в модели представления. После завершения процесса связывателя модели MVC, скопировать свойства модели представления экземпляра сущности, например, при необходимости используя средство [AutoMapper](http://automapper.org/). Используйте базу данных. Запись на экземпляр сущности для присвоено состояние Unchanged, а затем установите Property("PropertyName"). IsModified значение true для каждого свойства объекта, включенного в модель представления. Этот метод работает в обоих изменять и создавать сценарии.

    Кроме `Bind` атрибут, `try-catch` блок является единственным изменением, внесенные в код формирования шаблонов. Если исключение, которое является производным от [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) будет обработано, пока сохраняются изменения, отображаются сообщения об ошибке. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) исключения иногда вызываются объекты, внешние приложения, а не ошибку, поэтому пользователю предлагается повторите попытку. Несмотря на то, что не реализована в этом образце, производственного качества приложения будет занесения исключения. Дополнительные сведения см. в разделе **подробные сведения в журнале** статьи [мониторинг и данные телеметрии (облака реальных построение приложений с Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Код в *Views\Student\Create.cshtml* аналогична видели в *Details.cshtml*, за исключением того, что `EditorFor` и `ValidationMessageFor` вспомогательные методы, используемые для каждого поля, а не `DisplayFor`. Ниже приведен соответствующий код:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *CREATE.chstml* также включает `@Html.AntiForgeryToken()`, которая работает с `ValidateAntiForgeryToken` атрибут контроллере, чтобы предотвратить [межсайтовой подделки запросов](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) атак.

    Изменения не требуются в *Create.cshtml*.
2. Запустите страницу, выбрав **учащихся** и нажав кнопку **создать новый**.
3. Введите имена и обнаружении неверной даты и нажмите кнопку **создать** для сообщения об ошибке.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Это проверка на стороне сервера, получения по умолчанию; в этом руководстве вы увидите, как добавлять атрибуты, которые также создают код для проверки на стороне клиента. Следующий выделенный код показывает проверки модели в **создать** метод.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Измените значение на допустимое Дата и нажмите кнопку **создать** для просмотра нового студента, которые отображаются в **индекс** страницы.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Метод HttpPost изменения обновления

В *Controllers\StudentController.cs*, `HttpGet` `Edit` метод (один без `HttpPost` атрибут) использует `Find` метод для извлечения выбранного `Student` сущности, как вы узнали в `Details` метод. Измените этот метод не требуется.

Тем не менее, замените `HttpPost` `Edit` метод действия с помощью следующего кода:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Эти изменения реализации соображениям безопасности, чтобы предотвратить [оверпостинга](#overpost), scaffolder создан `Bind` атрибута и добавлены сущности, созданные связыватель модели для набора с флагом Modified сущностей. Том, что код больше не рекомендуется, поскольку `Bind` атрибут очищает все существующие данные в поля, которые не перечислены в `Include` параметра. В будущем, scaffolder контроллера MVC будут обновлены, чтобы он не генерирует `Bind` атрибутов для изменения методов.

Новый код считывает существующую сущность и вызовы [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) обновить поля из введенных пользователем данных в отправленной формы данных. Платформа Entity Framework автоматическое отслеживание наборов изменений [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) флаг для сущности. Когда [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) вызывается метод, `Modified` флаг заставляет Entity Framework для создания инструкции SQL для обновления строки базы данных. [Конфликты параллелизма](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) игнорируются, и все столбцы, строки базы данных обновляются, включая те, которые не были изменены пользователем. (Этом руководстве показано, как обрабатывать конфликты параллелизма, и если требуется только отдельные поля для обновления в базе данных, можно задать сущности неизменное и установить отдельные поля для изменения.)

Рекомендуется, чтобы предотвратить overposting, поля, которые должны быть обновляемым по странице, входят в `TryUpdateModel` параметров. В настоящее время нет дополнительных полей, вы защищаете, но список полей, которые должны связыватель модели для привязки гарантирует, что при добавлении поля в модели данных в будущем, автоматически защищены ли они пока не будут добавлены явно здесь.

В результате этих изменений сигнатуре метода HttpPost редактирования метода является метод edit HttpGet; Таким образом вы переименовали метод EditPost.

> [!TIP] 
> 
> **Состояния сущностей и Attach и методы SaveChanges**
> 
> Отслеживает отслеживания объекта контекст базы данных, будут ли сущностей в памяти, в соответствии с их соответствующих строк в базе данных, и эта информация определяет, что происходит при вызове `SaveChanges` метода. Например, при передаче новую сущность, [добавить](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) метод, который присваивается состояние сущности `Added`. Затем при вызове [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метода, контекст базы данных выдает SQL `INSERT` команды.
> 
> Сущность может находиться в одном из[следующие состояния](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`. Сущности в базе данных еще не существует. `SaveChanges` Метод должен выдать `INSERT` инструкции.
> - `Unchanged`. Не нужно ничего сделать с помощью этой сущности, `SaveChanges` метод. При чтении из базы данных сущности, сущность начинается с этим состоянием.
> - `Modified`. Были изменены некоторые или все значения свойств сущности. `SaveChanges` Метод должен выдать `UPDATE` инструкции.
> - `Deleted`. Сущность была помечена для удаления. `SaveChanges` Метод должен выдать `DELETE` инструкции.
> - `Detached`. Сущность не отслеживается контекст базы данных.
> 
> В приложении для настольных систем изменения состояния обычно устанавливаются автоматически. В приложении рабочего стола типа чтение сущности и изменить некоторые значения свойств. В результате состояние сущности автоматически меняется на `Modified`. Затем при вызове `SaveChanges`, Entity Framework создает SQL `UPDATE` инструкцию, которая обновляет только фактические свойства, которые были изменены.
> 
> Для этой непрерывной последовательности не допускает отключены от веб-приложений. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , читающего сущность удаляется после отрисовки страницы. При `HttpPost` `Edit` вызывается метод действия, новый запрос выполняется, и у вас есть новый экземпляр [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), поэтому необходимо вручную установить состояние сущности `Modified.` , а затем при вызове `SaveChanges`, Платформа Entity Framework обновляет все столбцы, строки базы данных, так как контекст никак не может знать, какие свойства были изменены.
> 
> Если требуется, чтобы SQL `Update` инструкцию, чтобы обновить только поля, которые пользователь действительно были изменены, можно сохранить первоначальные значения определенным образом (например, скрытые поля), чтобы они были доступны при `HttpPost` `Edit` вызывается метод. Затем вы можете создать `Student` сущности с помощью исходных значений, вызов `Attach` с этой версией исходной сущности, обновления значений сущности с новыми значениями и затем вызвать `SaveChanges.` Дополнительные сведения см. в разделе [ Состояния сущности и SaveChanges](https://msdn.microsoft.com/data/jj592676) и [локальные данные](https://msdn.microsoft.com/data/jj592872) в центре разработчиков MSDN данных.


Код HTML и Razor в *Views\Student\Edit.cshtml* аналогична видели в *Create.cshtml*, и изменения не требуются.

Запустите страницу, выбрав **учащихся** и затем нажав кнопку **изменить** гиперссылки.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Изменить некоторые данные, и нажмите кнопку **Сохранить**. Можно видеть данные, измененные страницы индекса.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Обновление страницы удаления

В *Controllers\StudentController.cs*, код шаблона для `HttpGet` `Delete` использует метод `Find` метод для извлечения выбранного `Student` сущности, как вы видели в `Details` и `Edit` методы. Тем не менее, для реализации пользовательской ошибки сообщение при вызове `SaveChanges` завершается ошибкой, мы добавим некоторые функциональные возможности этого метода и его соответствующее представление.

Как показано для обновления и создания операций, операций удаления может потребоваться два метода действия. Метод, который вызывается в ответ на запрос GET отображает представление, которое дает пользователю возможность утверждения или отменить операцию удаления. Если пользователь утверждает его, создается запрос POST. Когда это происходит, `HttpPost` `Delete` вызывается метод, а затем этот метод фактически выполняет операцию удаления.

Вы добавите `try-catch` заблокировать к `HttpPost` `Delete` метод для обработки всех ошибок, которые могут возникнуть при обновлении базы данных. При возникновении ошибки `HttpPost` `Delete` вызовы метода `HttpGet` `Delete` метод, передав ему параметр, указывающий, что произошла ошибка. `HttpGet Delete` Метод затем заново отобразит страницу подтверждения и сообщение об ошибке, предоставляя пользователю возможность отменить или повторить попытку.

1. Замените `HttpGet` `Delete` метод действия с следующий код, который управляет отчеты об ошибках: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Этот код принимает [необязательный параметр](https://msdn.microsoft.com/library/dd264739.aspx) , указывающее, является ли метод был вызван после сбоя, чтобы сохранить изменения. Этот параметр является `false` при `HttpGet` `Delete` метод вызывается без предыдущего сбоя. При вызове `HttpPost` `Delete` в ответ на ошибку обновления базы данных, параметр является `true` и сообщение об ошибке передается в представление.
- Замените `HttpPost` `Delete` метода действия (с именем `DeleteConfirmed`) следующим кодом, который выполняет операцию удаления фактическое и перехватывает все ошибки обновления базы данных.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Этот код извлекает выбранную сущность, затем вызывает метод [удалить](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) метод, чтобы задать состояния сущности `Deleted`. Когда `SaveChanges` вызывается SQL `DELETE` команда создается. Изменились имя метода действия из `DeleteConfirmed` для `Delete`. Формирования шаблонов код, называемый `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` метод уникальная сигнатура. (Среда CLR требуется перегруженные методы, чтобы иметь параметры другой метод). Теперь, когда сигнатурах являются уникальными, можно не покидайте соглашение MVC и использовать то же имя для `HttpPost` и `HttpGet` удаления методов.

    Если приоритетом является повышение производительности в приложении большого объема, чтобы избежать ненужных SQL-запрос для получения строки путем замены строки кода, которые вызывают `Find` и `Remove` методы следующим кодом:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Этот код создает экземпляр `Student` сущности с помощью только значения первичного ключа, а затем задает состояние сущности `Deleted`. Это все, что платформа Entity Framework требуется, чтобы удалить сущность.

    Как уже отмечалось, `HttpGet` `Delete` метода не удаляет данные. Выполнение операции delete в ответ на команду GET запроса (или неважно, выполнение любых операций изменения, создайте операции или любой другой операции, изменяющий данные) создает угрозу безопасности. Дополнительные сведения см. в разделе [46 совет # ASP.NET MVC — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Стивен Вальтер блога.
- В *Views\Student\Delete.cshtml*, добавить сообщение об ошибке между `h2` заголовок и `h3` заголовок, как показано в следующем примере:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

    Запустите страницу, выбрав **учащихся** и нажав кнопку **удалить** гиперссылки:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
- Нажмите кнопку **удалить**. Без удаленных студента отображается страница индекса. (Вы увидите пример код обработки ошибок в действие в [параллелизма учебника](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Закрытие подключения к базе данных

Закройте подключения к базе данных и освободить ресурсы, которые они содержат, как можно быстрее, освободить экземпляр контекста, когда вы завершили работу с ним. Т. е. Почему формирования шаблонов код предоставляет [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) метод в конце `StudentController` класса в *StudentController.cs*, как показано в следующем примере:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Базовый `Controller` уже класс реализует `IDisposable` интерфейс, поэтому этот код просто добавляет переопределение, чтобы `Dispose(bool)` метод при явном удалении экземпляра контекста.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Обработка транзакций

По умолчанию платформа Entity Framework неявно реализует транзакции. В сценариях, где можно внести изменения в несколько строк или таблиц, а затем вызвать `SaveChanges`, Entity Framework автоматически гарантирует, что все изменения, внесенные завершится успехом или ошибкой все. Если сначала выполняются некоторые изменения, а затем происходит ошибка, эти изменения автоматически происходит возврат. См. в сценариях, где требуется дополнительный контроль — например, если вы хотите включить действия, выполненные за пределами платформы Entity Framework транзакции-- [работы с транзакциями](https://msdn.microsoft.com/data/dn456843) на сайте MSDN.

## <a name="summary"></a>Сводка

Теперь у вас есть полный набор страниц, которые выполняют простые операции CRUD для `Student` сущностей. Вспомогательные методы MVC используется для создания элементов пользовательского интерфейса для полей данных. Дополнительные сведения о вспомогательных методов MVC см. в разделе [визуализации с помощью формы HTML Helpers](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (страницы является для MVC 3 но все еще актуальны для MVC 5).

В следующем уроке будет расширяют функциональные возможности страницы индекса, добавив сортировку и разбиение по страницам.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[Вперед](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
