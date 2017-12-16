---
title: "Страниц Razor с основными EF - CRUD - 2, 8."
author: rick-anderson
description: "Показано, как создание, чтение, обновление, удаление с основными EF"
keywords: "ASP.NET Core, Entity Framework Core, CRUD, создание, чтение, обновление, удаление"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Создания, чтения, обновления и удаления - Core EF со страницами Razor (2, 8)

По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

В этом учебнике формирования шаблонов CRUD (Создание, чтение, обновление и удаление) просматривается и настроить код.

Примечание: Чтобы уменьшить сложность и сохранить эти учебники, основное внимание уделено EF Core, EF основного кода используется в файлах кода страниц Razor. Некоторые разработчики используют шаблон службы уровня или репозитория в создание уровень абстракции между пользовательского интерфейса (страниц Razor) и уровень доступа к данным.

В этот учебник, создания, изменения, удаления и страниц Razor сведения в *студента* папки будут изменены.

Создание, изменение и удаление страниц, формирования шаблонов кода строится по следующему шаблону:

* Получение и отображение запрошенные данные с помощью метода HTTP GET `OnGetAsync`.
* Сохранить изменения в данных с помощью метода HTTP POST `OnPostAsync`.

На страницах индекса и сведения, получения и отображения запрошенные данные с помощью метода HTTP GET`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Замените SingleOrDefaultAsync FirstOrDefaultAsync

Созданный код использует [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) для выборки Запрошенная сущность. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) более эффективен при выборке одной сущности:

* Если код должен проверить, не более одной сущности, возвращенных запросом. 
* `SingleOrDefaultAsync`данные извлекаются и выполняет лишнюю работу.
* `SingleOrDefaultAsync`создает исключение, если имеется более одной сущности, которая соответствует часть фильтра.
*  `FirstOrDefaultAsync`не создавать при наличии более одной сущности, которая соответствует часть фильтра.

Замените глобально `SingleOrDefaultAsync` с `FirstOrDefaultAsync`. `SingleOrDefaultAsync`используется в 5 местах:

* `OnGetAsync`на странице «сведения».
* `OnGetAsync`и `OnPostAsync` на страницах, Edit и Delete.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

В основную часть формирования шаблонов кода [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) может использоваться вместо `FirstOrDefaultAsync` или `SingleOrDefaultAsync`. 

`FindAsync`:

* Находит сущности с первичным ключом (PK). Если сущность с PK отслеживается контекстом, она возвращается без запроса к базе данных.
* — Простой и быстрый.
* Оптимизирован для поиска одной сущности.
* Можно иметь преимущества производительности в некоторых ситуациях, но они редко следует учитывать для обычных веб-скриптов.
* Неявно использует [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) вместо [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Но если необходимо включить другие сущности, то поиск больше не подходит. Это означает, что может потребоваться отменить поиск и переместить запрос по мере продвижения вашего приложения.

## <a name="customize-the-details-page"></a>Настройка страницы сведений

Перейдите к `Pages/Students` страницы. **Изменить**, **сведения**, и **удаление** ссылки, созданные [вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в *страниц и студенты / Index.cshtml* файл.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Выберите ссылку сведения. URL-адрес имеет вид `http://localhost:5000/Students/Details?id=2`. Идентификатор студента передается с помощью строки запроса (`?id=2`).

Обновить редактирования, подробные сведения и удаление страниц Razor для использования `"{id:int}"` шаблон маршрута. Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`.

Запрос на странице с помощью шаблона маршрута «{ИД: int}», выполняющий **не** включают целое число со знаком маршрута значение возвращает HTTP 404 (не найдено) ошибку. Например `http://localhost:5000/Students/Details` возвращает ошибку 404. Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:

 ```cshtml
@page "{id:int?}"
```

Запустить приложение, щелкните ссылку сведений и проверьте URL-адрес передает идентификатор как данные маршрута (`http://localhost:5000/Students/Details/2`).

Не изменяйте глобально `@page` для `@page "{id:int}"`, это нарушит ссылки на корневую папку и создавать страницы.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Добавление связанных данных

Отсутствует код, формирования шаблонов для страницы индекса учащихся `Enrollments` свойство. В этом разделе содержимого `Enrollments` коллекция отображается на странице «сведения».

`OnGetAsync` Метод *Pages/Students/Details.cshtml.cs* использует `FirstOrDefaultAsync` метод для извлечения одной `Student` сущности. Добавьте следующий выделенный код:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

`Include` И `ThenInclude` методы, которые вызывают контекст загрузки `Student.Enrollments` свойство навигации и в пределах каждой регистрации `Enrollment.Course` свойство навигации. Эти методы являются examinied подробно в учебнике чтение связанных данных.

`AsNoTracking` Метод позволяет повысить производительность в сценариях, если возвращаются сущности, не обновляются в текущем контексте. `AsNoTracking`рассматривается далее в этом учебнике.

### <a name="display-related-enrollments-on-the-details-page"></a>Отображение связанных регистраций на этой странице

Откройте *Pages/Students/Details.cshtml*. Добавьте следующий выделенный код, чтобы отобразить список регистраций.

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Если коду отступ неправильный после вставки кода, нажмите клавиши CTRL-K-D, чтобы исправить его.

Приведенный выше код выполняет цикл по сущности в `Enrollments` свойство навигации. Для каждой регистрации отображает название курса и уровень. Название курса извлекается из курса сущность, которая хранится в `Course` свойства навигации сущности регистраций.

Запустите приложение, выберите **учащихся** и нажмите кнопку **сведения** ссылку для учащихся. Отображается список курсов и оценок для выбранного учащегося.

## <a name="update-the-create-page"></a>Обновление страницы создания

Обновление `OnPostAsync` метод в *Pages/Students/Create.cshtml.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Изучите [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) кода:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

В приведенном выше коде `TryUpdateModelAsync<Student>` пытается обновить `emptyStudent` объекта, используя значения из отправленной формы [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) свойство в [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync`обновляет только свойства, перечисленные (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

В предыдущем примере:

* Второй аргумент (` "student", // Prefix`) — это префикс использует для поиска значений. Не учитывается регистр символов.
* Значения из отправленной формы преобразуются в типы в `Student` модели с помощью [привязки модели](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Оверпостинга

С помощью `TryUpdateModel` для обновления полей с переданные значения является рекомендации по безопасности, так как он предотвращает overposting. Предположим, например, сущность Student включает `Secret` свойство, которое этой веб-страницы не следует обновить или добавить:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Даже если приложение не имеет `Secret` поля для создания или обновления страниц Razor, злоумышленник может установить `Secret` значение оверпостинга. Злоумышленник может использовать такое средство, как Fiddler или написать сценарий JavaScript для учета `Secret` формирования значения. Исходный код не ограничивать поля, которые использует связыватель модели при создании экземпляра студента.

Любое другое значение злоумышленнику, указанный для `Secret` поля формы обновляется в базе данных. На следующем рисунке показано добавление средство Fiddler `Secret` поле (со значением «OverPost») для значения из отправленной формы.

![При добавлении секретный поля Fiddler](../ef-mvc/crud/_static/fiddler.png)

Значение «OverPost» успешно добавлен `Secret` свойство вставленной строки. Конструктор приложения не предназначены `Secret` установить со страницы создания это свойство.

<a name="vm"></a>
### <a name="view-model"></a>Модель представления

Модель представления обычно содержит подмножество свойств, включенных в модель, используемые приложением. Модель приложения часто называют модели домена. Модель домена обычно содержит все свойства, необходимые для соответствующей сущности в базе данных. Модель представления содержит только свойства, необходимые для уровня пользовательского интерфейса (например, страницы создания). В дополнение к модели представления некоторые приложения используют привязки модели или модели ввода для передачи данных между классом кода программной страниц Razor и браузер. Рассмотрим следующий `Student` модель представления:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Просмотр моделей предоставляют альтернативный способ предотвратить overposting. Модель представления содержит только свойства для просмотра (отображение) или обновления.

В следующем коде используется `StudentVM` модель представления для создания нового студента:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) метод задает значения этого объекта, считывая значения из другого [объекты propertyvalue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) объекта. `SetValues`использует сопоставление по имени свойства. Тип модели представления может не быть связанным с типом модели, необходимо иметь свойства, которые соответствуют только.

С помощью `StudentVM` требует [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) обновить для использования `StudentVM` вместо `Student`.

На страницах Razor `PageModel` производный класс является модели представления. 

## <a name="update-the-edit-page"></a>Обновление страницы правки

Добавьте в файл кода страницы изменения:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Изменения в коде похожи на странице создания за некоторыми исключениями.

* `OnPostAsync`имеется необязательный `id` параметра.
* Текущий студента извлекаются из базы данных, вместо создания пустой студента.
* `FirstOrDefaultAsync`был заменен [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync`лучше всего подойдет при выборе сущности из первичного ключа. В разделе [FindAsync](#FindAsync) для получения дополнительной информации.

### <a name="test-the-edit-and-create-pages"></a>Тестирование редактирования и создания страниц

Создание и изменение сущностей несколько учащихся.

## <a name="entity-states"></a>Состояния сущностей

Сохраняет сведения о контекст базы данных, являются ли сущности в памяти в соответствии с их соответствующих строк в базе данных. Сведения о синхронизации контекста базы данных определяет, что происходит при `SaveChanges` вызывается. Например, при передаче объекта в `Add` метод, который присваивается состояние сущности `Added`. Когда `SaveChanges` вызывается DB контекста выдает команду SQL INSERT.

Сущность может иметь одно из следующих состояний:

* `Added`: Объект еще не существует в базе данных. `SaveChanges` Метод выдает инструкции INSERT.

* `Unchanged`: Никакие изменения должны быть сохранены с этой сущностью. Сущность находится в этом состоянии при чтении данных из базы данных.

* `Modified`: Были изменены некоторые или все значения свойств сущности. `SaveChanges` Метод выдает инструкции UPDATE.

* `Deleted`: Сущность была помечена для удаления. `SaveChanges` Метод выполняет инструкцию DELETE.

* `Detached`: Объект не отслеживается контекст базы данных.

В приложении рабочего стола изменения состояния обычно устанавливаются автоматически. Сущность для чтения, изменения и состояние сущности автоматически меняется на `Modified`. Вызов `SaveChanges` создает инструкцию SQL UPDATE, которая обновляет только измененных свойств.

В веб-приложении `DbContext` , осуществляющий чтение сущности и отображает данные удаляется после отрисовки страницы. Если страницы `OnPostAsync` вызывается метод, новый веб-запроса и для нового экземпляра `DbContext`. Повторное чтение сущности в этот новый контекст имитирует обработки рабочего стола.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

В этом разделе добавляется код для реализации пользовательской ошибки сообщение при вызове `SaveChanges` завершается ошибкой. Добавьте строку к possile сообщений об ошибке:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Замените метод `OnGetAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Предыдущий код содержит необязательный параметр `saveChangesError`. `saveChangesError`Указывает, является ли метод был вызван после сбоя, чтобы удалить объект student. Операция удаления может завершиться ошибкой из-за временных проблем с сетью. В облаке скорее временный сетевой ошибки. `saveChangesError`ложно, когда страница удаления `OnGetAsync` вызывается из пользовательского интерфейса. Когда `OnGetAsync` вызывается `OnPostAsync` (из-за сбоя операции удаления), `saveChangesError` параметр имеет значение true.

### <a name="the-delete-pages-onpostasync-method"></a>Метод OnPostAsync удаления страницы

Замените `OnPostAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Приведенный выше код извлекает выбранную сущность, затем вызывает метод `Remove` метод, чтобы задать состояния сущности `Deleted`. При `SaveChanges` вызове SQL DELETE команда создается. Если `Remove` завершается ошибкой:

* Порождено исключение базы данных.
* Удаление страниц `OnGetAsync` метод вызывается с `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Страница «обновление» Razor Delete

Добавьте сообщение об ошибке выделенный страницу удалить Razor.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Проверка удаления.

## <a name="common-errors"></a>Распространенные ошибки

Домашние студентов или другие ссылки не поддерживаются.

Проверьте страницу Razor содержит правильное `@page` директивы. Например, студентов или домашней странице Razor следует **не** содержать шаблон маршрута:

```cshtml
@page "{id:int}"
```

Каждая страница Razor должна включать `@page` директивы.

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/intro)
[Вперед](xref:data/ef-rp/sort-filter-page)
