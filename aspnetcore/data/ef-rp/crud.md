---
title: "Razor Pages с EF Core — операции CRUD — 2 из 8"
author: rick-anderson
description: "Описание операций создания, чтения, обновления и удаления в EF Core"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Создание, чтение, обновление и удаление — EF Core с Razor Pages (2 из 8)

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

В этом учебнике описывается проверка и настройка шаблонного кода операций CRUD (создание, чтение, обновление и удаление).

Примечание. Чтобы максимально упростить этот код и сконцентрироваться на работе с EF Core, в моделях страниц Razor Pages в этих руководствах используется код EF Core. Некоторые разработчики используют уровень служб или шаблон репозитория для создания уровня абстракции между пользовательским интерфейсом (Razor Pages) и уровнем доступа к данным.

В рамках этого учебника изменяются страницы Razor Pages в папке *Student*, предназначенные для создания, редактирования, удаления и просмотра сведений.

В шаблонном коде используется следующий шаблон для страниц создания, редактирования и удаления:

* Получение и отображение запрашиваемых данных с помощью метода HTTP GET`OnGetAsync`.
* Сохранение изменений в данных с помощью метода HTTP POST`OnPostAsync`.

Страницы указателя и сведений получают и отображают запрашиваемые данные с помощью метода HTTP GET`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Замена SingleOrDefaultAsync на FirstOrDefaultAsync

В созданном коде для выборки запрашиваемой сущности используется [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_). При выборке одной сущности более высокую эффективность демонстрирует [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_):

* Если в коде не требуется проверять, что запрос возвращает не более одной сущности. 
* `SingleOrDefaultAsync` выбирает больше данных и выполняет ненужные операции.
* `SingleOrDefaultAsync` вызывает исключение при наличии нескольких сущностей, соответствующих части фильтра.
*  `FirstOrDefaultAsync` на вызывает исключение при наличии нескольких сущностей, соответствующих части фильтра.

Выполните глобальную замену `SingleOrDefaultAsync` на `FirstOrDefaultAsync`. `SingleOrDefaultAsync` используется в 5 случаях:

* `OnGetAsync` на странице сведений.
* `OnGetAsync` и `OnPostAsync` на страницах редактирования и удаления.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

Чаще всего в шаблонном коде [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) можно использовать вместо `FirstOrDefaultAsync` или `SingleOrDefaultAsync`. 

`FindAsync`:

* Находит сущность с первичным ключом. Если сущность с первичным ключом отслеживается контекстом, она возвращается без запроса к базе данных.
* Является простым и быстрым.
* Оптимизирован для поиска одной сущности.
* В некоторых ситуациях может давать преимущества в производительности, однако редко используется в обычных веб-сценариях.
* Неявно использует [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) вместо [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Однако если необходимо включить другие сущности, результаты поиска более не будут подходить. Это значит, что по мере работы приложения может потребоваться отменить результаты поиска и перейти к запросу.

## <a name="customize-the-details-page"></a>Настройка страницы сведений

Перейдите на страницу `Pages/Students`. Ссылки **Edit**, **Details** и **Delete** создаются [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в файле *Pages/Students/Index.cshtml*.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Щелкните ссылку Details. URL-адрес имеет вид `http://localhost:5000/Students/Details?id=2`. Идентификатор учащегося передается с помощью строки запроса (`?id=2`).

Обновите страницы Razor Pages Edit, Details и Delete так, чтобы использовался шаблон маршрута `"{id:int}"`. Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`.

Запрос к странице с шаблоном маршрута "{id:int}", который **не** включает в себя целочисленное значение маршрута, приводит к ошибке HTTP 404 (не найдено). Например, `http://localhost:5000/Students/Details` возвращает ошибку 404. Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:

 ```cshtml
@page "{id:int?}"
```

Запустите приложение, щелкните ссылку Details и убедитесь, что в URL-адресе в виде данных маршрута передается идентификатор (`http://localhost:5000/Students/Details/2`).

Не выполняйте глобальную замену `@page` на `@page "{id:int}"`, поскольку это приведет к нарушению ссылок на страницы Home и Create.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Добавление связанных данных

Шаблонный код страницы указателя учащихся не включает свойство `Enrollments`. В этом разделе на странице Details отображается содержимое коллекции `Enrollments`.

Метод `OnGetAsync` в файле *Pages/Students/Details.cshtml.cs* использует метод `FirstOrDefaultAsync` для извлечения одной сущности `Student`. Добавьте выделенный ниже код:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

Методы `Include` и `ThenInclude` инструктируют контекст для загрузки свойства навигации `Student.Enrollments`, а также свойства навигации `Enrollment.Course` в пределах каждой регистрации. Эти методы более подробно рассматриваются в учебнике, посвященном чтению данных.

Метод `AsNoTracking` повышает производительность в тех сценариях, где возвращаемые сущности не обновляются в текущем контексте. `AsNoTracking` рассматривается позднее в этом учебнике.

### <a name="display-related-enrollments-on-the-details-page"></a>Отображение связанных регистраций на странице Details

Откройте файл *Pages/Students/Details.cshtml*. Добавьте выделенный ниже код, чтобы отобразить список регистраций:

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Если после вставки кода нарушаются отступы в нем, нажмите клавиши CTRL-K-D, чтобы исправить это.

Приведенный выше код циклически обрабатывает сущности в свойстве навигации `Enrollments`. Для каждой регистрации он отображает название курса и оценку. Название курса извлекается из сущности Course, которая хранится в свойстве навигации `Course` сущности Enrollments.

Запустите приложение, выберите вкладку **Students** (Учащиеся) и щелкните ссылку **Details** (Сведения) для учащегося. Отобразится список курсов и оценок для выбранного учащегося.

## <a name="update-the-create-page"></a>Обновление страницы Create

Измените метод `OnPostAsync` в файле *Pages/Students/Create.cshtml.cs*, используя следующий код:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Проверьте код [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

В приведенном выше коде `TryUpdateModelAsync<Student>` пытается обновить объект `emptyStudent`, используя отправленные значения формы из свойства [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) в [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync` обновляет только перечисленные свойства (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

В предыдущем примере:

* Второй аргумент (` "student", // Prefix`) представляет собой префикс для поиска значений. Задается без учета регистра символов.
* Отправленные значения формы преобразуются в типы в модели `Student` с использованием [привязки модели](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Чрезмерная передача данных

В целях повышения безопасности рекомендуется использовать `TryUpdateModel` для обновления полей на основе отправленных значений, поскольку в этом случае исключается чрезмерная передача данных. Например, сущность Student включает свойство `Secret`, которое веб-страница не должна обновлять или добавлять:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Даже если приложение не имеет поля `Secret` на странице создания или обновления Razor Pages, злоумышленник может установить значение `Secret` посредством чрезмерной передачи данных. Злоумышленник может использовать такие средства, как Fiddler, или собственный код JavaScript для отправки значения формы `Secret`. В исходном коде не ограничиваются поля, которые используются при создании экземпляра Student связывателем модели.

Какое бы значение ни задал злоумышленник для поля формы `Secret`, оно будет обновлено в базе данных. На следующем рисунке показано средство Fiddler, с помощью которого в отправленные значения формы добавляется поле `Secret` (со значением "OverPost").

![Добавление поля Secret с помощью средства Fiddler](../ef-mvc/crud/_static/fiddler.png)

Значение "OverPost" успешно добавлено в свойство `Secret` вставленной строки. Разработчик приложения не планировал, что свойство `Secret` будет устанавливаться на странице Create.

<a name="vm"></a>
### <a name="view-model"></a>Модель представления

Модель представления обычно содержит подмножество свойств, которые включены в модель и используются приложением. Модель приложения часто называют моделью домена. Модель домена обычно содержит все свойства, необходимые для соответствующей сущности в базе данных. Модель представления содержит только те свойства, которые необходимы уровню пользовательского интерфейса (например, на странице Create). Помимо модели представления в некоторых приложениях используется модель привязки или модель ввода для передачи данных между классом модели страницы Razor Pages и браузером. Рассмотрим следующую модель представления `Student`:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Модели представления реализуют альтернативный подход к защите от чрезмерной передачи данных. Модель представления содержит только свойства для просмотра (отображения) или обновления.

В следующем коде модель представления используется `StudentVM` для создания нового учащегося:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

Метод [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) устанавливает значения этого объекта, считывая значения из другого объекта [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues). `SetValues` использует сопоставление имен свойств. Тип модели представления может быть не связан с типом модели, однако они должны содержать совпадающие свойства.

При использовании `StudentVM` необходимо обновить [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml), чтобы использовать `StudentVM` вместо `Student`.

В Razor Pages представление модели реализуется с помощью производного класса `PageModel`. 

## <a name="update-the-edit-page"></a>Обновление страницы редактирования

Обновите модель страницы для страницы Edit:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Изменения в коде аналогичны странице Create за некоторыми исключениями:

* `OnPostAsync` имеет необязательный параметр `id`.
* Текущий учащийся извлекается из базы данных вместо того, чтобы создавать нового учащегося.
* `FirstOrDefaultAsync` заменен на [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync` лучше использовать при выборе сущности из первичного ключа. Дополнительные сведения см. в разделе [FindAsync](#FindAsync).

### <a name="test-the-edit-and-create-pages"></a>Проверка страниц редактирования и создания

Создайте и измените несколько сущностей учащихся.

## <a name="entity-states"></a>Состояния сущностей

Контекст базы данных отслеживает синхронизацию сущностей в памяти с соответствующими им строками в базе данных. Сведения о синхронизации контекста базы данных определяют поведение при вызове метода `SaveChanges`. Например, при передаче новой сущности в метод `Add` ей присваивается состояние `Added`. При вызове метода `SaveChanges` контекст базы данных выполняет команду SQL INSERT.

Возможны следующие состояния сущности:

* `Added`: сущность еще не существует в базе данных. Метод `SaveChanges` выполняет инструкцию INSERT.

* `Unchanged`: никакие изменения сущности не сохраняются. Сущность находится в этом состоянии при считывании из базы данных.

* `Modified`: были изменены значения некоторых или всех свойств сущности. Метод `SaveChanges` выполняет инструкцию UPDATE.

* `Deleted`: сущность отмечена для удаления. Метод `SaveChanges` выполняет инструкцию DELETE.

* `Detached`: сущность не отслеживается контекстом базы данных.

В классическом приложении изменения состояния обычно осуществляются автоматически. После считывания сущности и ее изменения ей автоматически присваивается состояние `Modified`. При вызове метода `SaveChanges` создается инструкция SQL UPDATE, которая обновляет только измененные свойства.

В веб-приложении объект `DbContext`, который считывает сущность и отображает ее данные, ликвидируется после отрисовки страницы. При вызове метода страниц `OnPostAsync` выполняется новый веб-запрос с новым экземпляром `DbContext`. Если повторно считать сущность в этот новый контекст, таким образом будет смоделирована обработка в классическом приложении.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

В этом разделе добавляется код, реализующий настраиваемое сообщение ошибки на случай сбоя при вызове `SaveChanges`. Добавьте строку, содержащую возможные сообщения об ошибке:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Замените метод `OnGetAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Приведенный выше код содержит необязательный параметр `saveChangesError`. `saveChangesError` указывает, был ли метод вызван после того, как произошел сбой при удалении объекта учащегося. Операция удаления может завершиться сбоем из-за временных проблем с сетью. Вероятность возникновения временных проблем с сетью выше в облаке. `saveChangesError` имеет значение false при вызове `OnGetAsync` страницы Delete из пользовательского интерфейса. Если `OnGetAsync` вызывается методом `OnPostAsync` (из-за сбоя операции удаления), параметру `saveChangesError` присваивается значение true.

### <a name="the-delete-pages-onpostasync-method"></a>Метод OnPostAsync страницы Delete

Замените `OnPostAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Приведенный выше код извлекает выбранную сущность и вызывает метод `Remove`, чтобы присвоить ей состояние `Deleted`. При вызове метода `SaveChanges` создается инструкция SQL DELETE. В случае сбоя `Remove`:

* Вызывается исключение базы данных.
* Вызывается метод `OnGetAsync` страницы Delete с параметром `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Обновление страницы удаления Razor Pages

Добавьте выделенное ниже сообщение об ошибке на страницу Delete Razor Pages.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Проверьте удаление.

## <a name="common-errors"></a>Распространенные ошибки

Не работает ссылка Student/Home или другие ссылки:

Убедитесь, что на странице Razor Pages содержится правильная директива `@page`. Например, страница Razor Pages Student/Home **не должна** содержать шаблон маршрута:

```cshtml
@page "{id:int}"
```

Каждая страница Razor Pages должна содержать директиву `@page`.

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/intro)
[Вперед](xref:data/ef-rp/sort-filter-page)
