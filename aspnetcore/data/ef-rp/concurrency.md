---
title: "Страниц Razor с основными EF - параллелизма - 8, 8."
author: rick-anderson
description: "Этот учебник показывает, как для обработки конфликтов, когда нескольким пользователям обновлять ту же сущность в то же время."
keywords: "ASP.NET Core, Entity Framework Core, параллелизма"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 0c49376fd1b602fe03ef2a152d19b58513ae2710
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2017
---
en-us /

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Обработка конфликтов параллелизма - Core EF со страницами Razor (8 8)

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), и [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Этот учебник показывает, как для обработки конфликтов, когда нескольким пользователям обновлять сущность одновременно (в то же время). Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда:

* Пользователь переходит на страницу редактирования для сущности.
* Другой пользователь обновляет той же сущности перед изменение первый пользователь будет записано в базу данных.

Если обнаружение параллелизма не включено, при выполнении параллельных обновлений:

* Последнее обновление wins. То есть последние обновления значения сохраняются в базы данных.
* Первый из текущего обновления, будут потеряны.

### <a name="optimistic-concurrency"></a>Оптимистический параллелизм

Оптимистичного параллелизма позволяет конфликтов параллелизма активна и затем реакцию соответствующим образом об их появлении. Например Мария — страница отдела редактирования и изменяет бюджет для английского языка отдела с $350,000.00 на 0,00 долларов.

![Изменение бюджета 0](concurrency/_static/change-budget.png)

Перед щелкает Мария **Сохранить**, Джон посещает той же странице и изменяет поле Дата начала 9/1/2007 9/1/2013.

![Изменение даты начала 2013](concurrency/_static/change-date.png)

Мария щелкает **Сохранить** первый и видит ее изменить, когда браузер отображает страницу индекса.

![Бюджет изменено на ноль](concurrency/_static/budget-zero.png)

Джон выбирает **Сохранить** на страницу редактирования, которая по-прежнему показывает бюджет 350,000.00 $. Дальнейший ход событий определяется порядок обработки конфликтов параллелизма.

Оптимистический параллелизм включает следующие параметры:

* Можно хранить список какие свойства были изменены пользователем и обновлять соответствующие столбцы в базе данных.

 В этом сценарии данные не будут потеряны. Различные свойства были обновлены двумя пользователями. При очередном кто-то просматривает отделе английского языка, они видят Джейн и Джона изменения. Этот метод обновления может снизить количество конфликтов, которые могут привести к потере данных. Этот подход: * нельзя избежать потери данных, если конкурирующих изменений для одного свойства.
        * — Обычно не удобно в веб-приложения. Он требует поддержки значительные состояния, чтобы отслеживать все извлеченных и новые значения. Обслуживание больших объемов состояния может повлиять на производительность приложения.
        * Может увеличить сложность приложений по сравнению с обнаружения параллелизма для сущности.

* Можно разрешить изменение Джона перезаписать Джейн изменений.

 Далее время кто-то просматривает отделе английского языка, они видят 9/1/2013 и извлеченное значение $350,000.00. Такой подход называется *клиент побеждает* или *побеждает последний* сценария. (Все значения из клиента имеют приоритет над возможности хранилища данных). Если этого не сделать, написания кода для обработки параллелизма, клиент побеждает происходит автоматически.

* Можно запретить изменение Джона обновляются в базе данных. Как правило, приложение будет: * сообщение об ошибке.
        * Показывают текущее состояние данных.
        * Разрешить пользователю повторно примените изменения.

 Это называется *побеждает хранилища* сценария. (Значения хранилища данных имеют приоритет над значениями, отправленные клиентом). В этом учебнике реализовать сценарий Wins хранилища. Этот метод гарантирует, что изменения не будут перезаписаны без оповещения пользователя.

## <a name="handling-concurrency"></a>Обработка параллелизма 

Если свойство настроен как [маркер параллелизма](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):

* EF Core проверяет, что свойство не был изменен после его, которые были выбраны. Проверка будет выполняться при [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) или [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) вызывается.
* Если свойство было изменено после получено, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) возникает исключение. 

Модели базы данных и данных должен быть настроен для поддержки генерации `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Обнаружение конфликтов параллелизма, свойство

Конфликты параллелизма, которые могут быть обнаружены на уровне свойств с [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) атрибута. Атрибут может применяться с несколькими свойствами в модели. Дополнительные сведения см. в разделе [данных заметок-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` Атрибут не используется в этом учебнике.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Обнаружение конфликтов параллелизма в строке

Для обнаружения конфликтов параллелизма [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) столбец отслеживания добавляется к модели.  `rowversion` :

* Зависит от SQL Server. Другие базы данных может не предоставляют подобную функцию.
* Используется для определения того, что сущность не был изменен после загрузки данных из базы данных. 

Базы данных приводит к возникновению ошибки последовательный `rowversion` номер, увеличивающееся каждый раз строка обновляется. В `Update` или `Delete` команды `Where` предложение включает извлеченное значение из `rowversion`. Если выполняется обновление строки были изменены.

 * `rowversion`не соответствует извлеченное значение.
 * `Update` Или `Delete` команд не найти строку, так как `Where` предложение включает извлеченных `rowversion`.
 * Объект `DbUpdateConcurrencyException` создается исключение.

В ядре EF, если строки не были обновлены с `Update` или `Delete` команды, возникает исключение параллелизма.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Добавление свойства отслеживания в сущности «отдел»

В *Models/Department.cs*, добавьте RowVersion следящее свойство:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) атрибут указывает, что этот столбец включен в `Where` предложения `Update` и `Delete` команд. Атрибут называется `Timestamp` из-за предыдущих версий SQL Server используется SQL `timestamp` типа данных перед SQL `rowversion` тип заменил его.

Fluent API можно также задать свойство отслеживания:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

В следующем коде показано часть EF Core, созданные при обновлении название отдела T-SQL:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Предыдущий выделяются показывает кода `WHERE` предложение, содержащее `RowVersion`. Если базы данных `RowVersion` не равен `RowVersion` параметра (`@p2`), не будут обновлены.

Следующий выделенный код показано T-SQL, который подтверждает, что только одна строка была обновлена.

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) возвращает количество строк, затронутых при выполнении последней инструкции. Невозможно строки обновляются, вызывает EF Core `DbUpdateConcurrencyException`.

Вы увидите ядро EF T-SQL создает в окне вывода Visual Studio.

### <a name="update-the-db"></a>Обновление базы данных

Добавление `RowVersion` изменение свойств модели базы данных, которая требует миграции.

Выполните построение проекта. В окне командной строки введите следующую команду:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Указанные выше команды:

* Добавляет *миграций / {stamp}_RowVersion.cs время* файл для миграции.
* Обновления *Migrations/SchoolContextModelSnapshot.cs* файла. Обновление добавляет следующий выделенный код в `BuildModel` метод:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Выполняется миграция для обновления базы данных.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Формировать модели подразделения

* Выйдите из Visual Studio.
* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

Предыдущий закрепление команда `Department` модели. Откройте проект в Visual Studio.

Выполните построение проекта. Сборки приводит к возникновению ошибки следующим образом:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Глобальная замена `_context.Department` для `_context.Departments` (то есть, добавить «s» `Department`). 7 вхождений обнаружены и обновлены.

### <a name="update-the-departments-index-page"></a>Обновление страницы индекса отделов

Подсистема формирование шаблонов создан `RowVersion` столбец для страницы индекса, но это поле не должно отображаться. В этом учебнике, последний байт `RowVersion` отображается для понимания параллелизма. Последний байт не гарантированно уникален. Настоящее приложение не будет отображать `RowVersion` или последнего байта `RowVersion`.

Обновление страницы индекса:

* Замените индекс отделов.
* Замените на разметку, содержащую `RowVersion` с последнего байта `RowVersion`.
* Замените FirstMidName FullName.

В следующем примере показана обновленная страница:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Обновление модели редактирования страниц

Обновление *pages\departments\edit.cshtml.cs* следующим кодом:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Для обнаружения проблемы параллелизма [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) обновляется `rowVersion` значения из сущности указано, оно получено. EF Core создает команду SQL UPDATE с предложением WHERE, содержащее исходное `RowVersion` значение. Если нет строк, затронутых командой обновления (не строки имеют исходное `RowVersion` значение), `DbUpdateConcurrencyException` исключение.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

В приведенном выше коде `Department.RowVersion` является значением, если сущности, которые были выбраны. `OriginalValue`значение в базе данных при `FirstOrDefaultAsync` в этот метод был вызван.

Следующий код возвращает значения клиента (и значений, переданных этому методу) и значения базы данных:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

В следующем коде добавляет пользовательское сообщение об ошибке для каждого столбца, имеющего DB значения, отличные от что отправления `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Следующие выделяются наборов кодов `RowVersion` получить значение новое значение из базы данных. В следующий раз при щелчке **Сохранить**, только те ошибки параллелизма, которые происходят с момента последнего отображения страницы правки будет перехвачено.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` Инструкция является обязательным, поскольку `ModelState` имеет старый `RowVersion` значение. На странице Razor `ModelState` значение для поля имеет приоритет над значения свойств модели, если установлены оба.

## <a name="update-the-edit-page"></a>Обновление страницы правки

Обновление *Pages/Departments/Edit.cshtml* следующей разметкой:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Выше разметка:

* Обновления `page` директив из `@page` для `@page "{id:int}"`.
* Добавляет версию скрытых строк. `RowVersion`Поэтому обратной передачи привязывает значение необходимо добавить.
* Отображает последний байт `RowVersion` для целей отладки.
* Заменяет `ViewData` со строго типизированными `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Тестирование конфликтов параллелизма со страницы правки

Откройте два браузеры изменения в отделе английского языка:

* Запустите приложение и выберите отделов.
* Щелкните правой кнопкой мыши **изменить** гиперссылку для английского языка подразделение и выберите **открыть в новой вкладке**.
* На первой вкладке щелкните **изменить** гиперссылки для английского языка отдела.

Браузер две вкладки те же сведения.

Измените имя на первой вкладке браузера и нажмите кнопку **Сохранить**.

![Изменение подразделения страница 1 после изменения](concurrency/_static/edit-after-change-1.png)

В браузере будет отображена страница индекса с обновленной rowVersion индикатора и измененное значение. Индикатор обновленные rowVersion, обратите внимание, он отображается на второй обратной передачи на вкладке "другие".

Изменение различные поля на второй вкладке браузера.

![Изменение подразделения страница 2 после изменения](concurrency/_static/edit-after-change-2.png)

Нажмите кнопку **Сохранить**. Вы видите сообщения об ошибках для всех полей, которые не соответствуют значениям базы данных:

![Сообщение об ошибке отдел изменить страницу](concurrency/_static/edit-error.png)

Это окно браузера не собираетесь изменить поля «имя». Скопируйте и вставьте значение текущей (языки) в поле «имя». Нажатием клавиши TAB out. Проверка на стороне клиента удаляет сообщение об ошибке.

![Сообщение об ошибке отдел изменить страницу](concurrency/_static/cv.png)

Нажмите кнопку **Сохранить** еще раз. Сохраняется значение, введенное на второй вкладке браузера. Вы видите сохраненные значения страницы индекса.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

Обновление модели страницы удаления со следующим кодом:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Страница удаления обнаруживает конфликты параллелизма при изменении сущности после его, которые были выбраны. `Department.RowVersion`представляет версию строки при сущности, которые были выбраны. Когда основные EF создает команду SQL DELETE, он включает предложение WHERE с `RowVersion`. Если повлияет на результаты команду SQL DELETE в 0 строк.

* `RowVersion` В SQL DELETE не соответствует команда `RowVersion` в базе данных.
* DbUpdateConcurrencyException исключение.
* `OnGetAsync`вызывается с `concurrencyError`.

### <a name="update-the-delete-page"></a>Обновление страницы удаления

Обновление *Pages/Departments/Delete.cshtml* следующим кодом:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Предыдущей разметки вносит следующие изменения:

* Обновления `page` директив из `@page` для `@page "{id:int}"`.
* Добавляет сообщение об ошибке.
* Заменяет FirstMidName FullName в **администратора** поля.
* Изменения `RowVersion` для отображения последнего байта.
* Добавляет версию скрытых строк. `RowVersion`Поэтому обратной передачи привязывает значение необходимо добавить.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Тестирование конфликтов параллелизма со страницей Delete

Создайте подразделение теста.

Откройте два браузеры Delete от отдела тестирования:

* Запустите приложение и выберите отделов.
* Щелкните правой кнопкой мыши **удаление** гиперссылку для отдела тестирования и выберите **открыть в новой вкладке**.
* Нажмите кнопку **изменить** гиперссылки для отдела тестирования.

Браузер две вкладки те же сведения.

Измените бюджет на первой вкладке браузера и нажмите кнопку **Сохранить**.

В браузере будет отображена страница индекса с обновленной rowVersion индикатора и измененное значение. Индикатор обновленные rowVersion, обратите внимание, он отображается на второй обратной передачи на вкладке "другие".

Удалите отдела тестирования со второй закладки. Ошибки параллелизма отображается с текущими значениями данных из базы данных. Щелкнув **удаление** удаляет сущность, если не `RowVersion` было updated.department был удален.

В разделе [наследования](xref:data/ef-mvc/inheritance) инструкции о том, как наследование в модели данных.

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Маркеры параллелизма EF ядра](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [Обработка параллелизма EF ядра](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/update-related-data)
