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
ms.openlocfilehash: 841c638b2cacaab7970f2b173fee488972957b63
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2018
---
<span data-ttu-id="34185-104">en-us /</span><span class="sxs-lookup"><span data-stu-id="34185-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="34185-105">Обработка конфликтов параллелизма - Core EF со страницами Razor (8 8)</span><span class="sxs-lookup"><span data-stu-id="34185-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="34185-106">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), и [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="34185-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="34185-107">Этот учебник показывает, как для обработки конфликтов, когда нескольким пользователям обновлять сущность одновременно (в то же время).</span><span class="sxs-lookup"><span data-stu-id="34185-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="34185-108">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="34185-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="34185-109">Конфликты параллелизма</span><span class="sxs-lookup"><span data-stu-id="34185-109">Concurrency conflicts</span></span>

<span data-ttu-id="34185-110">Конфликт параллелизма возникает, когда:</span><span class="sxs-lookup"><span data-stu-id="34185-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="34185-111">Пользователь переходит на страницу редактирования для сущности.</span><span class="sxs-lookup"><span data-stu-id="34185-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="34185-112">Другой пользователь обновляет той же сущности перед изменение первый пользователь будет записано в базу данных.</span><span class="sxs-lookup"><span data-stu-id="34185-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="34185-113">Если обнаружение параллелизма не включено, при выполнении параллельных обновлений:</span><span class="sxs-lookup"><span data-stu-id="34185-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="34185-114">Последнее обновление wins.</span><span class="sxs-lookup"><span data-stu-id="34185-114">The last update wins.</span></span> <span data-ttu-id="34185-115">То есть последние обновления значения сохраняются в базы данных.</span><span class="sxs-lookup"><span data-stu-id="34185-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="34185-116">Первый из текущего обновления, будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="34185-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="34185-117">Оптимистическая блокировка</span><span class="sxs-lookup"><span data-stu-id="34185-117">Optimistic concurrency</span></span>

<span data-ttu-id="34185-118">Оптимистичного параллелизма позволяет конфликтов параллелизма активна и затем реакцию соответствующим образом об их появлении.</span><span class="sxs-lookup"><span data-stu-id="34185-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="34185-119">Например Мария — страница отдела редактирования и изменяет бюджет для английского языка отдела с $350,000.00 на 0,00 долларов.</span><span class="sxs-lookup"><span data-stu-id="34185-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Изменение бюджета 0](concurrency/_static/change-budget.png)

<span data-ttu-id="34185-121">Перед щелкает Мария **Сохранить**, Джон посещает той же странице и изменяет поле Дата начала 9/1/2007 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="34185-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Изменение даты начала 2013](concurrency/_static/change-date.png)

<span data-ttu-id="34185-123">Мария щелкает **Сохранить** первый и видит ее изменить, когда браузер отображает страницу индекса.</span><span class="sxs-lookup"><span data-stu-id="34185-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Бюджет изменено на ноль](concurrency/_static/budget-zero.png)

<span data-ttu-id="34185-125">Джон выбирает **Сохранить** на страницу редактирования, которая по-прежнему показывает бюджет 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="34185-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="34185-126">Дальнейший ход событий определяется порядок обработки конфликтов параллелизма.</span><span class="sxs-lookup"><span data-stu-id="34185-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="34185-127">Оптимистический параллелизм включает следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="34185-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="34185-128">Можно хранить список какие свойства были изменены пользователем и обновлять соответствующие столбцы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="34185-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="34185-129">В этом сценарии данные не будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="34185-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="34185-130">Различные свойства были обновлены двумя пользователями.</span><span class="sxs-lookup"><span data-stu-id="34185-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="34185-131">При очередном кто-то просматривает отделе английского языка, они видят Джейн и Джона изменения.</span><span class="sxs-lookup"><span data-stu-id="34185-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="34185-132">Этот метод обновления может снизить количество конфликтов, которые могут привести к потере данных.</span><span class="sxs-lookup"><span data-stu-id="34185-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="34185-133">Этот подход: * нельзя избежать потери данных, если конкурирующих изменений для одного свойства.</span><span class="sxs-lookup"><span data-stu-id="34185-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="34185-134">* — Обычно не удобно в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="34185-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="34185-135">Он требует поддержки значительные состояния, чтобы отслеживать все извлеченных и новые значения.</span><span class="sxs-lookup"><span data-stu-id="34185-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="34185-136">Обслуживание больших объемов состояния может повлиять на производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="34185-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="34185-137">* Может увеличить сложность приложений по сравнению с обнаружения параллелизма для сущности.</span><span class="sxs-lookup"><span data-stu-id="34185-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="34185-138">Можно разрешить изменение Джона перезаписать Джейн изменений.</span><span class="sxs-lookup"><span data-stu-id="34185-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="34185-139">Далее время кто-то просматривает отделе английского языка, они видят 9/1/2013 и извлеченное значение $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="34185-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="34185-140">Такой подход называется *клиент побеждает* или *побеждает последний* сценария.</span><span class="sxs-lookup"><span data-stu-id="34185-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="34185-141">(Все значения из клиента имеют приоритет над возможности хранилища данных). Если этого не сделать, написания кода для обработки параллелизма, клиент побеждает происходит автоматически.</span><span class="sxs-lookup"><span data-stu-id="34185-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="34185-142">Можно запретить изменение Джона обновляются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="34185-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="34185-143">Как правило, приложение будет: * сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="34185-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="34185-144">* Показывают текущее состояние данных.</span><span class="sxs-lookup"><span data-stu-id="34185-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="34185-145">* Разрешить пользователю повторно примените изменения.</span><span class="sxs-lookup"><span data-stu-id="34185-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="34185-146">Это называется *побеждает хранилища* сценария.</span><span class="sxs-lookup"><span data-stu-id="34185-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="34185-147">(Значения хранилища данных имеют приоритет над значениями, отправленные клиентом). В этом учебнике реализовать сценарий Wins хранилища.</span><span class="sxs-lookup"><span data-stu-id="34185-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="34185-148">Этот метод гарантирует, что изменения не будут перезаписаны без оповещения пользователя.</span><span class="sxs-lookup"><span data-stu-id="34185-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="34185-149">Обработка параллелизма</span><span class="sxs-lookup"><span data-stu-id="34185-149">Handling concurrency</span></span> 

<span data-ttu-id="34185-150">Если свойство настроен как [маркер параллелизма](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="34185-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="34185-151">EF Core проверяет, что свойство не был изменен после его, которые были выбраны.</span><span class="sxs-lookup"><span data-stu-id="34185-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="34185-152">Проверка будет выполняться при [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) или [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) вызывается.</span><span class="sxs-lookup"><span data-stu-id="34185-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="34185-153">Если свойство было изменено после получено, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="34185-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="34185-154">Модели базы данных и данных должен быть настроен для поддержки генерации `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="34185-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="34185-155">Обнаружение конфликтов параллелизма, свойство</span><span class="sxs-lookup"><span data-stu-id="34185-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="34185-156">Конфликты параллелизма, которые могут быть обнаружены на уровне свойств с [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) атрибута.</span><span class="sxs-lookup"><span data-stu-id="34185-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="34185-157">Атрибут может применяться с несколькими свойствами в модели.</span><span class="sxs-lookup"><span data-stu-id="34185-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="34185-158">Дополнительные сведения см. в разделе [данных заметок-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="34185-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="34185-159">`[ConcurrencyCheck]` Атрибут не используется в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="34185-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="34185-160">Обнаружение конфликтов параллелизма в строке</span><span class="sxs-lookup"><span data-stu-id="34185-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="34185-161">Для обнаружения конфликтов параллелизма [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) столбец отслеживания добавляется к модели.</span><span class="sxs-lookup"><span data-stu-id="34185-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="34185-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="34185-162">`rowversion` :</span></span>

* <span data-ttu-id="34185-163">Зависит от SQL Server.</span><span class="sxs-lookup"><span data-stu-id="34185-163">Is SQL Server specific.</span></span> <span data-ttu-id="34185-164">Другие базы данных может не предоставляют подобную функцию.</span><span class="sxs-lookup"><span data-stu-id="34185-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="34185-165">Используется для определения того, что сущность не был изменен после загрузки данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="34185-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="34185-166">Базы данных приводит к возникновению ошибки последовательный `rowversion` номер, увеличивающееся каждый раз строка обновляется.</span><span class="sxs-lookup"><span data-stu-id="34185-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="34185-167">В `Update` или `Delete` команды `Where` предложение включает извлеченное значение из `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="34185-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="34185-168">Если выполняется обновление строки были изменены.</span><span class="sxs-lookup"><span data-stu-id="34185-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="34185-169">`rowversion`не соответствует извлеченное значение.</span><span class="sxs-lookup"><span data-stu-id="34185-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="34185-170">`Update` Или `Delete` команд не найти строку, так как `Where` предложение включает извлеченных `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="34185-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="34185-171">Объект `DbUpdateConcurrencyException` создается исключение.</span><span class="sxs-lookup"><span data-stu-id="34185-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="34185-172">В ядре EF, если строки не были обновлены с `Update` или `Delete` команды, возникает исключение параллелизма.</span><span class="sxs-lookup"><span data-stu-id="34185-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="34185-173">Добавление свойства отслеживания в сущности «отдел»</span><span class="sxs-lookup"><span data-stu-id="34185-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="34185-174">В *Models/Department.cs*, добавьте RowVersion следящее свойство:</span><span class="sxs-lookup"><span data-stu-id="34185-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="34185-175">[Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) атрибут указывает, что этот столбец включен в `Where` предложения `Update` и `Delete` команд.</span><span class="sxs-lookup"><span data-stu-id="34185-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="34185-176">Атрибут называется `Timestamp` из-за предыдущих версий SQL Server используется SQL `timestamp` типа данных перед SQL `rowversion` тип заменил его.</span><span class="sxs-lookup"><span data-stu-id="34185-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="34185-177">Fluent API можно также задать свойство отслеживания:</span><span class="sxs-lookup"><span data-stu-id="34185-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="34185-178">В следующем коде показано часть EF Core, созданные при обновлении название отдела T-SQL:</span><span class="sxs-lookup"><span data-stu-id="34185-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="34185-179">Предыдущий выделяются показывает кода `WHERE` предложение, содержащее `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="34185-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="34185-180">Если базы данных `RowVersion` не равен `RowVersion` параметра (`@p2`), не будут обновлены.</span><span class="sxs-lookup"><span data-stu-id="34185-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="34185-181">Следующий выделенный код показано T-SQL, который подтверждает, что только одна строка была обновлена.</span><span class="sxs-lookup"><span data-stu-id="34185-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="34185-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) возвращает количество строк, затронутых при выполнении последней инструкции.</span><span class="sxs-lookup"><span data-stu-id="34185-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="34185-183">Невозможно строки обновляются, вызывает EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="34185-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="34185-184">Вы увидите ядро EF T-SQL создает в окне вывода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34185-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="34185-185">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="34185-185">Update the DB</span></span>

<span data-ttu-id="34185-186">Добавление `RowVersion` изменение свойств модели базы данных, которая требует миграции.</span><span class="sxs-lookup"><span data-stu-id="34185-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="34185-187">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="34185-187">Build the project.</span></span> <span data-ttu-id="34185-188">В окне командной строки введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="34185-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="34185-189">Указанные выше команды:</span><span class="sxs-lookup"><span data-stu-id="34185-189">The preceding commands:</span></span>

* <span data-ttu-id="34185-190">Добавляет *миграций / {stamp}_RowVersion.cs время* файл для миграции.</span><span class="sxs-lookup"><span data-stu-id="34185-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="34185-191">Обновления *Migrations/SchoolContextModelSnapshot.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="34185-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="34185-192">Обновление добавляет следующий выделенный код в `BuildModel` метод:</span><span class="sxs-lookup"><span data-stu-id="34185-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="34185-193">Выполняется миграция для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="34185-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="34185-194">Формировать модели подразделения</span><span class="sxs-lookup"><span data-stu-id="34185-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="34185-195">Выйдите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34185-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="34185-196">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="34185-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="34185-197">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="34185-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="34185-198">Предыдущий закрепление команда `Department` модели.</span><span class="sxs-lookup"><span data-stu-id="34185-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="34185-199">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34185-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="34185-200">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="34185-200">Build the project.</span></span> <span data-ttu-id="34185-201">Сборки приводит к возникновению ошибки следующим образом:</span><span class="sxs-lookup"><span data-stu-id="34185-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="34185-202">Глобальная замена `_context.Department` для `_context.Departments` (то есть, добавить «s» `Department`).</span><span class="sxs-lookup"><span data-stu-id="34185-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="34185-203">7 вхождений обнаружены и обновлены.</span><span class="sxs-lookup"><span data-stu-id="34185-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="34185-204">Обновление страницы индекса отделов</span><span class="sxs-lookup"><span data-stu-id="34185-204">Update the Departments Index page</span></span>

<span data-ttu-id="34185-205">Подсистема формирование шаблонов создан `RowVersion` столбец для страницы индекса, но это поле не должно отображаться.</span><span class="sxs-lookup"><span data-stu-id="34185-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="34185-206">В этом учебнике, последний байт `RowVersion` отображается для понимания параллелизма.</span><span class="sxs-lookup"><span data-stu-id="34185-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="34185-207">Последний байт не гарантированно уникален.</span><span class="sxs-lookup"><span data-stu-id="34185-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="34185-208">Настоящее приложение не будет отображать `RowVersion` или последнего байта `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="34185-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="34185-209">Обновление страницы индекса:</span><span class="sxs-lookup"><span data-stu-id="34185-209">Update the Index page:</span></span>

* <span data-ttu-id="34185-210">Замените индекс отделов.</span><span class="sxs-lookup"><span data-stu-id="34185-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="34185-211">Замените на разметку, содержащую `RowVersion` с последнего байта `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="34185-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="34185-212">Замените FirstMidName FullName.</span><span class="sxs-lookup"><span data-stu-id="34185-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="34185-213">В следующем примере показана обновленная страница:</span><span class="sxs-lookup"><span data-stu-id="34185-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="34185-214">Обновление модели редактирования страниц</span><span class="sxs-lookup"><span data-stu-id="34185-214">Update the Edit page model</span></span>

<span data-ttu-id="34185-215">Обновление *pages\departments\edit.cshtml.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="34185-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="34185-216">Для обнаружения проблемы параллелизма [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) обновляется `rowVersion` значения из сущности, оно получено.</span><span class="sxs-lookup"><span data-stu-id="34185-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="34185-217">EF Core создает команду SQL UPDATE с предложением WHERE, содержащее исходное `RowVersion` значение.</span><span class="sxs-lookup"><span data-stu-id="34185-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="34185-218">Если нет строк, затронутых командой обновления (не строки имеют исходное `RowVersion` значение), `DbUpdateConcurrencyException` исключение.</span><span class="sxs-lookup"><span data-stu-id="34185-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="34185-219">В приведенном выше коде `Department.RowVersion` является значением, если сущности, которые были выбраны.</span><span class="sxs-lookup"><span data-stu-id="34185-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="34185-220">`OriginalValue`значение в базе данных при `FirstOrDefaultAsync` в этот метод был вызван.</span><span class="sxs-lookup"><span data-stu-id="34185-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="34185-221">Следующий код возвращает значения клиента (и значений, переданных этому методу) и значения базы данных:</span><span class="sxs-lookup"><span data-stu-id="34185-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="34185-222">В следующем коде добавляет пользовательское сообщение об ошибке для каждого столбца, имеющего DB значения, отличные от что отправления `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="34185-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="34185-223">Следующие выделяются наборов кодов `RowVersion` получить значение новое значение из базы данных.</span><span class="sxs-lookup"><span data-stu-id="34185-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="34185-224">В следующий раз при щелчке **Сохранить**, только те ошибки параллелизма, которые происходят с момента последнего отображения страницы правки будет перехвачено.</span><span class="sxs-lookup"><span data-stu-id="34185-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="34185-225">`ModelState.Remove` Инструкция является обязательным, поскольку `ModelState` имеет старый `RowVersion` значение.</span><span class="sxs-lookup"><span data-stu-id="34185-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="34185-226">На странице Razor `ModelState` значение для поля имеет приоритет над значения свойств модели, если установлены оба.</span><span class="sxs-lookup"><span data-stu-id="34185-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="34185-227">Обновление страницы правки</span><span class="sxs-lookup"><span data-stu-id="34185-227">Update the Edit page</span></span>

<span data-ttu-id="34185-228">Обновление *Pages/Departments/Edit.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="34185-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="34185-229">Выше разметка:</span><span class="sxs-lookup"><span data-stu-id="34185-229">The preceding markup:</span></span>

* <span data-ttu-id="34185-230">Обновления `page` директив из `@page` для `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="34185-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="34185-231">Добавляет версию скрытых строк.</span><span class="sxs-lookup"><span data-stu-id="34185-231">Adds a hidden row version.</span></span> <span data-ttu-id="34185-232">`RowVersion`Поэтому обратной передачи привязывает значение необходимо добавить.</span><span class="sxs-lookup"><span data-stu-id="34185-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="34185-233">Отображает последний байт `RowVersion` для целей отладки.</span><span class="sxs-lookup"><span data-stu-id="34185-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="34185-234">Заменяет `ViewData` со строго типизированными `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="34185-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="34185-235">Тестирование конфликтов параллелизма со страницы правки</span><span class="sxs-lookup"><span data-stu-id="34185-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="34185-236">Откройте два браузеры изменения в отделе английского языка:</span><span class="sxs-lookup"><span data-stu-id="34185-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="34185-237">Запустите приложение и выберите отделов.</span><span class="sxs-lookup"><span data-stu-id="34185-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="34185-238">Щелкните правой кнопкой мыши **изменить** гиперссылку для английского языка подразделение и выберите **открыть в новой вкладке**.</span><span class="sxs-lookup"><span data-stu-id="34185-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="34185-239">На первой вкладке щелкните **изменить** гиперссылки для английского языка отдела.</span><span class="sxs-lookup"><span data-stu-id="34185-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="34185-240">Браузер две вкладки те же сведения.</span><span class="sxs-lookup"><span data-stu-id="34185-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="34185-241">Измените имя на первой вкладке браузера и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="34185-241">Change the name in the first browser tab and click **Save**.</span></span>

![Изменение подразделения страница 1 после изменения](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="34185-243">В браузере будет отображена страница индекса с обновленной rowVersion индикатора и измененное значение.</span><span class="sxs-lookup"><span data-stu-id="34185-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="34185-244">Индикатор обновленные rowVersion, обратите внимание, он отображается на второй обратной передачи на вкладке "другие".</span><span class="sxs-lookup"><span data-stu-id="34185-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="34185-245">Изменение различные поля на второй вкладке браузера.</span><span class="sxs-lookup"><span data-stu-id="34185-245">Change a different field in the second browser tab.</span></span>

![Изменение подразделения страница 2 после изменения](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="34185-247">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="34185-247">Click **Save**.</span></span> <span data-ttu-id="34185-248">Вы видите сообщения об ошибках для всех полей, которые не соответствуют значениям базы данных:</span><span class="sxs-lookup"><span data-stu-id="34185-248">You see error messages for all fields that don't match the DB values:</span></span>

![Сообщение об ошибке отдел изменить страницу](concurrency/_static/edit-error.png)

<span data-ttu-id="34185-250">Это окно браузера не собираетесь изменить поля «имя».</span><span class="sxs-lookup"><span data-stu-id="34185-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="34185-251">Скопируйте и вставьте значение текущей (языки) в поле «имя».</span><span class="sxs-lookup"><span data-stu-id="34185-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="34185-252">Нажатием клавиши TAB out. Проверка на стороне клиента удаляет сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="34185-252">Tab out. Client-side validation removes the error message.</span></span>

![Сообщение об ошибке отдел изменить страницу](concurrency/_static/cv.png)

<span data-ttu-id="34185-254">Нажмите кнопку **Сохранить** еще раз.</span><span class="sxs-lookup"><span data-stu-id="34185-254">Click **Save** again.</span></span> <span data-ttu-id="34185-255">Сохраняется значение, введенное на второй вкладке браузера.</span><span class="sxs-lookup"><span data-stu-id="34185-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="34185-256">Вы видите сохраненные значения страницы индекса.</span><span class="sxs-lookup"><span data-stu-id="34185-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="34185-257">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="34185-257">Update the Delete page</span></span>

<span data-ttu-id="34185-258">Обновление модели страницы удаления со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="34185-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="34185-259">Страница удаления обнаруживает конфликты параллелизма при изменении сущности после его, которые были выбраны.</span><span class="sxs-lookup"><span data-stu-id="34185-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="34185-260">`Department.RowVersion`представляет версию строки при сущности, которые были выбраны.</span><span class="sxs-lookup"><span data-stu-id="34185-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="34185-261">Когда основные EF создает команду SQL DELETE, он включает предложение WHERE с `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="34185-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="34185-262">Если повлияет на результаты команду SQL DELETE в 0 строк.</span><span class="sxs-lookup"><span data-stu-id="34185-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="34185-263">`RowVersion` В SQL DELETE не соответствует команда `RowVersion` в базе данных.</span><span class="sxs-lookup"><span data-stu-id="34185-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="34185-264">DbUpdateConcurrencyException исключение.</span><span class="sxs-lookup"><span data-stu-id="34185-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="34185-265">`OnGetAsync`вызывается с `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="34185-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="34185-266">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="34185-266">Update the Delete page</span></span>

<span data-ttu-id="34185-267">Обновление *Pages/Departments/Delete.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="34185-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="34185-268">Предыдущей разметки вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="34185-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="34185-269">Обновления `page` директив из `@page` для `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="34185-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="34185-270">Добавляет сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="34185-270">Adds an error message.</span></span>
* <span data-ttu-id="34185-271">Заменяет FirstMidName FullName в **администратора** поля.</span><span class="sxs-lookup"><span data-stu-id="34185-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="34185-272">Изменения `RowVersion` для отображения последнего байта.</span><span class="sxs-lookup"><span data-stu-id="34185-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="34185-273">Добавляет версию скрытых строк.</span><span class="sxs-lookup"><span data-stu-id="34185-273">Adds a hidden row version.</span></span> <span data-ttu-id="34185-274">`RowVersion`Поэтому обратной передачи привязывает значение необходимо добавить.</span><span class="sxs-lookup"><span data-stu-id="34185-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="34185-275">Тестирование конфликтов параллелизма со страницей Delete</span><span class="sxs-lookup"><span data-stu-id="34185-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="34185-276">Создайте подразделение теста.</span><span class="sxs-lookup"><span data-stu-id="34185-276">Create a test department.</span></span>

<span data-ttu-id="34185-277">Откройте два браузеры Delete от отдела тестирования:</span><span class="sxs-lookup"><span data-stu-id="34185-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="34185-278">Запустите приложение и выберите отделов.</span><span class="sxs-lookup"><span data-stu-id="34185-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="34185-279">Щелкните правой кнопкой мыши **удаление** гиперссылку для отдела тестирования и выберите **открыть в новой вкладке**.</span><span class="sxs-lookup"><span data-stu-id="34185-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="34185-280">Нажмите кнопку **изменить** гиперссылки для отдела тестирования.</span><span class="sxs-lookup"><span data-stu-id="34185-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="34185-281">Браузер две вкладки те же сведения.</span><span class="sxs-lookup"><span data-stu-id="34185-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="34185-282">Измените бюджет на первой вкладке браузера и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="34185-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="34185-283">В браузере будет отображена страница индекса с обновленной rowVersion индикатора и измененное значение.</span><span class="sxs-lookup"><span data-stu-id="34185-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="34185-284">Индикатор обновленные rowVersion, обратите внимание, он отображается на второй обратной передачи на вкладке "другие".</span><span class="sxs-lookup"><span data-stu-id="34185-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="34185-285">Удалите отдела тестирования со второй закладки. Ошибки параллелизма отображается с текущими значениями данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="34185-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="34185-286">Щелкнув **удаление** удаляет сущность, если не `RowVersion` было updated.department был удален.</span><span class="sxs-lookup"><span data-stu-id="34185-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="34185-287">В разделе [наследования](xref:data/ef-mvc/inheritance) о том, как наследовать модели данных.</span><span class="sxs-lookup"><span data-stu-id="34185-287">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="34185-288">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="34185-288">Additional resources</span></span>

* [<span data-ttu-id="34185-289">Маркеры параллелизма EF ядра</span><span class="sxs-lookup"><span data-stu-id="34185-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="34185-290">Обработка параллелизма EF ядра</span><span class="sxs-lookup"><span data-stu-id="34185-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="34185-291">Назад</span><span class="sxs-lookup"><span data-stu-id="34185-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
