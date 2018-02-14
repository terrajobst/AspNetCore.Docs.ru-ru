---
title: "Razor Pages с EF Core — параллелизм — 8 из 8"
author: rick-anderson
description: "Это руководство описывает, как обрабатывать конфликты, когда несколько пользователей одновременно изменяют одну сущность."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2018
---
<span data-ttu-id="1bf66-103">ru-ru/</span><span class="sxs-lookup"><span data-stu-id="1bf66-103">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="1bf66-104">Обработка конфликтов параллелизма — EF Core с Razor Pages (8 из 8)</span><span class="sxs-lookup"><span data-stu-id="1bf66-104">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="1bf66-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith)</span><span class="sxs-lookup"><span data-stu-id="1bf66-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="1bf66-106">Это руководство описывает, как обрабатывать конфликты, когда несколько пользователей параллельно (одновременно) изменяют одну сущность.</span><span class="sxs-lookup"><span data-stu-id="1bf66-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="1bf66-107">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="1bf66-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="1bf66-108">Конфликты параллелизма</span><span class="sxs-lookup"><span data-stu-id="1bf66-108">Concurrency conflicts</span></span>

<span data-ttu-id="1bf66-109">Конфликт параллелизма возникает в следующих условиях:</span><span class="sxs-lookup"><span data-stu-id="1bf66-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="1bf66-110">Пользователь переходит на страницу редактирования для сущности.</span><span class="sxs-lookup"><span data-stu-id="1bf66-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="1bf66-111">Другой пользователь обновляет ту же сущность до того, как изменение первого пользователя записывается в базу данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="1bf66-112">Если обнаружение параллелизма отключено, возникают параллельные изменения:</span><span class="sxs-lookup"><span data-stu-id="1bf66-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="1bf66-113">Побеждает последнее изменение.</span><span class="sxs-lookup"><span data-stu-id="1bf66-113">The last update wins.</span></span> <span data-ttu-id="1bf66-114">Таким образом, в базу данных заносятся значения из последнего изменения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="1bf66-115">Первые из текущих изменений утрачиваются.</span><span class="sxs-lookup"><span data-stu-id="1bf66-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="1bf66-116">Оптимистическая блокировка</span><span class="sxs-lookup"><span data-stu-id="1bf66-116">Optimistic concurrency</span></span>

<span data-ttu-id="1bf66-117">Оптимистическая блокировка допускает появление конфликтов параллелизма, а затем обрабатывает их соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="1bf66-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="1bf66-118">Например, Мария посещает страницу изменения кафедры и изменяет бюджет кафедры английской языка с 350 000,00 USD на 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="1bf66-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Изменение бюджета на 0](concurrency/_static/change-budget.png)

<span data-ttu-id="1bf66-120">Прежде чем Мария нажимает кнопку **Save** (Сохранить), Дмитрий заходит на ту же страницу и изменяет значение в поле "Start Date" (Дата начала) с 9/1/2007 на 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="1bf66-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Изменение начальной даты на 2013 г.](concurrency/_static/change-date.png)

<span data-ttu-id="1bf66-122">Сначала Мария нажимает кнопку **Save** (Сохранить) и видит свое изменение, когда браузер отображает страницу индекса.</span><span class="sxs-lookup"><span data-stu-id="1bf66-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Бюджет изменен на нуль](concurrency/_static/budget-zero.png)

<span data-ttu-id="1bf66-124">Дмитрий нажимает кнопку **Save** (Сохранить) на странице редактирования, где все еще отображается бюджет 350 000,00 USD.</span><span class="sxs-lookup"><span data-stu-id="1bf66-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="1bf66-125">Дальнейший ход событий определяется порядком обработки конфликтов параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1bf66-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="1bf66-126">Оптимистическая блокировка включает в себя следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="1bf66-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="1bf66-127">Вы можете отслеживать, для какого свойства пользователь изменил и обновил только соответствующие столбцы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="1bf66-128">В этом сценарии никакие данные не теряются.</span><span class="sxs-lookup"><span data-stu-id="1bf66-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="1bf66-129">Разные свойства были обновлены двумя пользователями.</span><span class="sxs-lookup"><span data-stu-id="1bf66-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="1bf66-130">Когда какой-либо пользователь просмотрит кафедру английского языка в следующий раз, он увидит изменения, внесенные как Марией, так и Дмитрием.</span><span class="sxs-lookup"><span data-stu-id="1bf66-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="1bf66-131">Этот способ обновления позволяет снизить количество конфликтов, способных привести к потере данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="1bf66-132">Этот подход имеет следующие особенности. Во-первых, он не позволяет избежать потери данных, если конкурирующие изменения вносятся для одного свойства.</span><span class="sxs-lookup"><span data-stu-id="1bf66-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="1bf66-133">Во-вторых, он не слишком хорошо подходит для веб-приложений,</span><span class="sxs-lookup"><span data-stu-id="1bf66-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="1bf66-134">так как требует поддерживать значительный объем состояний, чтобы отслеживать все извлеченные и новые значения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="1bf66-135">Обслуживание большого объема состояний может негативно повлиять на производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="1bf66-136">В-третьих, он может повысить сложность приложений по сравнению с обнаружением параллелизма для сущности.</span><span class="sxs-lookup"><span data-stu-id="1bf66-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="1bf66-137">Вы можете позволить изменению Дмитрия перезаписать изменение Марии.</span><span class="sxs-lookup"><span data-stu-id="1bf66-137">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="1bf66-138">Когда какой-либо пользователь просмотрит кафедру английского языка в следующий раз, он увидит дату 9/1/2013 и значение 350 000,00 USD.</span><span class="sxs-lookup"><span data-stu-id="1bf66-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="1bf66-139">Такой подход называется *победой клиента* или *сохранением последнего внесенного изменения*.</span><span class="sxs-lookup"><span data-stu-id="1bf66-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="1bf66-140">(Все значения из клиента имеют приоритет над данными в хранилище.) Если не писать код для обработки параллелизма, автоматически применяется победа клиента.</span><span class="sxs-lookup"><span data-stu-id="1bf66-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="1bf66-141">Вы можете запретить обновление изменения Дмитрия в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="1bf66-142">В этом случае приложение обычно отображает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="1bf66-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="1bf66-143">\* Отображает текущее состояние данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="1bf66-144">\* Разрешает пользователю повторно применить изменения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-144">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="1bf66-145">Это называется *победой хранилища*.</span><span class="sxs-lookup"><span data-stu-id="1bf66-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="1bf66-146">(Значения в хранилище имеют приоритет над данными, передаваемыми клиентом.) В этом руководстве вы реализуете сценарий победы хранилища.</span><span class="sxs-lookup"><span data-stu-id="1bf66-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="1bf66-147">Данный метод гарантирует, что никакие изменения не перезаписываются без оповещения пользователя.</span><span class="sxs-lookup"><span data-stu-id="1bf66-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="1bf66-148">Обработка параллелизма</span><span class="sxs-lookup"><span data-stu-id="1bf66-148">Handling concurrency</span></span> 

<span data-ttu-id="1bf66-149">Если свойство настроено как [токен параллелизма](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="1bf66-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="1bf66-150">EF Core проверяет, что свойство не было изменено после его получения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="1bf66-151">Эта проверка выполняется при вызове [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) или [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="1bf66-151">The check occurs when [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="1bf66-152">Если свойство было изменено после получения, возникает исключение [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1bf66-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="1bf66-153">Нужно настроить базу данных и модель данных для поддержки исключения `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="1bf66-154">Обнаружение конфликтов параллелизма для свойства</span><span class="sxs-lookup"><span data-stu-id="1bf66-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="1bf66-155">Конфликты параллелизма можно обнаружить на уровне свойств с помощью атрибута [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1bf66-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="1bf66-156">Этот атрибут можно применить к нескольким свойствам в модели.</span><span class="sxs-lookup"><span data-stu-id="1bf66-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="1bf66-157">Дополнительные сведения см. в [описании ConcurrencyCheck в подразделе "Заметки к данным"](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="1bf66-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="1bf66-158">Атрибут`[ConcurrencyCheck]` в этом руководстве не используется.</span><span class="sxs-lookup"><span data-stu-id="1bf66-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="1bf66-159">Обнаружение конфликтов параллелизма для строки</span><span class="sxs-lookup"><span data-stu-id="1bf66-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="1bf66-160">Чтобы обнаружить конфликты параллелизма, в модель добавлен столбец отслеживания [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="1bf66-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="1bf66-161">`rowversion`:</span><span class="sxs-lookup"><span data-stu-id="1bf66-161">`rowversion` :</span></span>

* <span data-ttu-id="1bf66-162">Относится к SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1bf66-162">Is SQL Server specific.</span></span> <span data-ttu-id="1bf66-163">Другие базы данных могут не предоставлять подобную функцию.</span><span class="sxs-lookup"><span data-stu-id="1bf66-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="1bf66-164">Используется для определения того, что сущность не была изменена после ее получения из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="1bf66-165">База данных формирует последовательный номер `rowversion`, увеличивающийся при каждом обновлении строки.</span><span class="sxs-lookup"><span data-stu-id="1bf66-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="1bf66-166">В команде `Update` или `Delete` предложение `Where` включает извлеченное значение из `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="1bf66-167">Если обновляемая строка изменились:</span><span class="sxs-lookup"><span data-stu-id="1bf66-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="1bf66-168">`rowversion` не соответствует полученному значению.</span><span class="sxs-lookup"><span data-stu-id="1bf66-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="1bf66-169">Команда `Update` или `Delete` не находит строку, так как предложение `Where` включает полученное значение `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="1bf66-170">Возникает исключение `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="1bf66-171">Когда в EF Core нет строк, обновленных командой `Update` или `Delete`, возникает исключение параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1bf66-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="1bf66-172">Добавление свойства отслеживания в сущность Department</span><span class="sxs-lookup"><span data-stu-id="1bf66-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="1bf66-173">В *Models/Department.cs* добавьте свойство отслеживания RowVersion:</span><span class="sxs-lookup"><span data-stu-id="1bf66-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="1bf66-174">Атрибут [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) указывает, что этот столбец входит в предложение `Where` для команд `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="1bf66-175">Этот атрибут называется `Timestamp`, так как предыдущие версии SQL Server использовали тип данных `timestamp` SQL, пока его не сменил тип `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="1bf66-176">Текучий API также может задавать свойство отслеживания:</span><span class="sxs-lookup"><span data-stu-id="1bf66-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="1bf66-177">В следующем коде показана часть кода T-SQL, созданного EF Core при обновлении названия кафедры:</span><span class="sxs-lookup"><span data-stu-id="1bf66-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="1bf66-178">Предыдущий выделенный код показывает предложение `WHERE`, содержащее `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="1bf66-179">Если база данных `RowVersion` не соответствует параметру `RowVersion` (`@p2`), никакие строки не обновляются.</span><span class="sxs-lookup"><span data-stu-id="1bf66-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="1bf66-180">Следующий выделенный код показывает код T-SQL, который подтверждает, что была обновлена всего одна строка.</span><span class="sxs-lookup"><span data-stu-id="1bf66-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="1bf66-181">[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) возвращает число строк, затронутых при выполнении последнего оператора.</span><span class="sxs-lookup"><span data-stu-id="1bf66-181">[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="1bf66-182">Если никакие строки не обновляются, EF Core выдает исключение `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="1bf66-183">Вы можете просмотреть код T-SQL, создаваемый EF Core в окне вывода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bf66-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="1bf66-184">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="1bf66-184">Update the DB</span></span>

<span data-ttu-id="1bf66-185">Добавление свойства `RowVersion` изменяет модель базы данных, которая требует миграции.</span><span class="sxs-lookup"><span data-stu-id="1bf66-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="1bf66-186">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="1bf66-186">Build the project.</span></span> <span data-ttu-id="1bf66-187">Введите в командном окне следующее:</span><span class="sxs-lookup"><span data-stu-id="1bf66-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="1bf66-188">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="1bf66-188">The preceding commands:</span></span>

* <span data-ttu-id="1bf66-189">Добавляет файл миграций *Migrations/{метка времени}_RowVersion.cs*.</span><span class="sxs-lookup"><span data-stu-id="1bf66-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="1bf66-190">Изменяет файл *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="1bf66-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="1bf66-191">Это изменение добавляет следующий выделенный код в метод `BuildModel`:</span><span class="sxs-lookup"><span data-stu-id="1bf66-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="1bf66-192">Запускает миграции для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="1bf66-193">Формирование шаблона для модели кафедр</span><span class="sxs-lookup"><span data-stu-id="1bf66-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="1bf66-194">Закройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bf66-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="1bf66-195">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="1bf66-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1bf66-196">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1bf66-196">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="1bf66-197">Предыдущая команда формирует шаблон для модели `Department`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="1bf66-198">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bf66-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="1bf66-199">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="1bf66-199">Build the project.</span></span> <span data-ttu-id="1bf66-200">Эта сборка выдает ошибки, например следующего характера:</span><span class="sxs-lookup"><span data-stu-id="1bf66-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="1bf66-201">Выполните глобальную замену `_context.Department` на `_context.Departments` (она заключается в добавлении "s" к `Department`).</span><span class="sxs-lookup"><span data-stu-id="1bf66-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="1bf66-202">Обнаруживаются и изменяются 7 экземпляров.</span><span class="sxs-lookup"><span data-stu-id="1bf66-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="1bf66-203">Изменение страницы индекса кафедр</span><span class="sxs-lookup"><span data-stu-id="1bf66-203">Update the Departments Index page</span></span>

<span data-ttu-id="1bf66-204">Подсистема формирования шаблонов создала столбец `RowVersion` для страницы индекса, однако это поле не должно отображаться.</span><span class="sxs-lookup"><span data-stu-id="1bf66-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="1bf66-205">В этом руководстве отображается последний байт `RowVersion` для лучшего понимания параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1bf66-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="1bf66-206">Этот последний байт необязательно является уникальным.</span><span class="sxs-lookup"><span data-stu-id="1bf66-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="1bf66-207">Реальное приложение не будет отображать `RowVersion` или последний байт `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="1bf66-208">Обновите страницу индекса:</span><span class="sxs-lookup"><span data-stu-id="1bf66-208">Update the Index page:</span></span>

* <span data-ttu-id="1bf66-209">Замените Index на Departments.</span><span class="sxs-lookup"><span data-stu-id="1bf66-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="1bf66-210">Замените разметку, содержащую `RowVersion`, на последний байт `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="1bf66-211">Замените FirstMidName на FullName.</span><span class="sxs-lookup"><span data-stu-id="1bf66-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="1bf66-212">Следующая разметка показывает обновленную страницу:</span><span class="sxs-lookup"><span data-stu-id="1bf66-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="1bf66-213">Обновление модели страницы "Edit" (Редактирование)</span><span class="sxs-lookup"><span data-stu-id="1bf66-213">Update the Edit page model</span></span>

<span data-ttu-id="1bf66-214">Измените *pages\departments\edit.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="1bf66-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="1bf66-215">Для обнаружения проблемы параллелизма [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) обновляется с помощью значения `rowVersion` из сущности, откуда он был получен.</span><span class="sxs-lookup"><span data-stu-id="1bf66-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="1bf66-216">EF Core создает команду SQL UPDATE с предложением WHERE, содержащим исходное значение `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="1bf66-217">Если команда UPDATE не затрагивает никакие строки (нет строк, имеющих исходное значение `RowVersion`), возникает исключение `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="1bf66-218">В приведенном выше коде `Department.RowVersion` является значением на момент извлечения сущности.</span><span class="sxs-lookup"><span data-stu-id="1bf66-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="1bf66-219">`OriginalValue` является значением в базе данных на момент вызова `FirstOrDefaultAsync` в этом методе.</span><span class="sxs-lookup"><span data-stu-id="1bf66-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="1bf66-220">Следующий код возвращает значения клиента (значения, переданные в этот метод) и значения базы данных:</span><span class="sxs-lookup"><span data-stu-id="1bf66-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="1bf66-221">Следующий код добавляет пользовательское сообщение об ошибке для каждого столбца, у которого значения базы данных отличаются в переданных в `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="1bf66-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="1bf66-222">Следующий выделенный код задает для `RowVersion` новое значение, полученное из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="1bf66-223">Когда пользователь в следующий раз нажимает кнопку **Save** (Сохранить), перехватываются только те ошибки параллелизма, возникшие с момента последнего отображения страницы "Edit" (Редактирование).</span><span class="sxs-lookup"><span data-stu-id="1bf66-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="1bf66-224">Оператор `ModelState.Remove` является обязательным, так как `ModelState` имеет старое значение `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="1bf66-225">На странице Razor значение `ModelState` для поля имеет приоритет над значениями свойств модели, если они присутствуют вместе.</span><span class="sxs-lookup"><span data-stu-id="1bf66-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="1bf66-226">Обновление страницы редактирования</span><span class="sxs-lookup"><span data-stu-id="1bf66-226">Update the Edit page</span></span>

<span data-ttu-id="1bf66-227">Обновите *Pages/Departments/Edit.cshtml*, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="1bf66-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="1bf66-228">Предыдущая разметка:</span><span class="sxs-lookup"><span data-stu-id="1bf66-228">The preceding markup:</span></span>

* <span data-ttu-id="1bf66-229">Изменяет директиву `page` с `@page` на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="1bf66-230">Добавляет версию скрытых строк.</span><span class="sxs-lookup"><span data-stu-id="1bf66-230">Adds a hidden row version.</span></span> <span data-ttu-id="1bf66-231">Нужно добавить `RowVersion`, чтобы обратная передача привязывала значение.</span><span class="sxs-lookup"><span data-stu-id="1bf66-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="1bf66-232">Отображает последний байт `RowVersion` в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="1bf66-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="1bf66-233">Заменяет `ViewData` на строго типизированный `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="1bf66-234">Тестирование конфликтов параллелизма с использованием страницы "Edit" (Редактирование)</span><span class="sxs-lookup"><span data-stu-id="1bf66-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="1bf66-235">Откройте в браузере два экземпляра страницы "Edit" (Редактирование) для кафедры английского языка:</span><span class="sxs-lookup"><span data-stu-id="1bf66-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="1bf66-236">Запустите приложение и выберите "Departments" (Кафедры).</span><span class="sxs-lookup"><span data-stu-id="1bf66-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="1bf66-237">Щелкните правой кнопкой мыши гиперссылку **Edit** (Изменить) для кафедры английского языка и выберите пункт **Открыть на новой вкладке**.</span><span class="sxs-lookup"><span data-stu-id="1bf66-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="1bf66-238">На первой вкладке щелкните гиперссылку **Edit** (Изменить) для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="1bf66-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="1bf66-239">На обеих вкладках браузера отображаются одинаковые сведения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="1bf66-240">Измените имя на первой вкладке браузера и нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="1bf66-240">Change the name in the first browser tab and click **Save**.</span></span>

![Страница "Edit" (Редактирование) кафедры 1 после изменения](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="1bf66-242">В браузере отображается страница индекса с измененным значением и обновленным индикатором rowVersion.</span><span class="sxs-lookup"><span data-stu-id="1bf66-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="1bf66-243">Обратите внимание на обновленный индикатор rowVersion, он отображается при второй обратной передаче на другой вкладке.</span><span class="sxs-lookup"><span data-stu-id="1bf66-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="1bf66-244">Измените другое поле на второй вкладке браузера.</span><span class="sxs-lookup"><span data-stu-id="1bf66-244">Change a different field in the second browser tab.</span></span>

![Страница "Edit" (Редактирование) кафедры 2 после изменения](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="1bf66-246">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="1bf66-246">Click **Save**.</span></span> <span data-ttu-id="1bf66-247">Отображаются сообщения об ошибках для всех полей, которые не соответствуют значениям базы данных:</span><span class="sxs-lookup"><span data-stu-id="1bf66-247">You see error messages for all fields that don't match the DB values:</span></span>

![Сообщение об ошибке для страницы "Edit" (Редактирование) кафедры](concurrency/_static/edit-error.png)

<span data-ttu-id="1bf66-249">В этом окне браузера не планировалось изменять поле "Name" (Имя).</span><span class="sxs-lookup"><span data-stu-id="1bf66-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="1bf66-250">Скопируйте и вставьте текущее значение "Languages" (Языки) в поле "Name" (Имя).</span><span class="sxs-lookup"><span data-stu-id="1bf66-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="1bf66-251">Выйдите из поля с помощью клавиши TAB. Проверка на стороне клиента удаляет сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="1bf66-251">Tab out. Client-side validation removes the error message.</span></span>

![Сообщение об ошибке для страницы "Edit" (Редактирование) кафедры](concurrency/_static/cv.png)

<span data-ttu-id="1bf66-253">Снова нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="1bf66-253">Click **Save** again.</span></span> <span data-ttu-id="1bf66-254">Сохраняется значение, введенное на второй вкладке браузера.</span><span class="sxs-lookup"><span data-stu-id="1bf66-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="1bf66-255">Сохраненные значения отображаются на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="1bf66-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="1bf66-256">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="1bf66-256">Update the Delete page</span></span>

<span data-ttu-id="1bf66-257">Обновите страницу "Delete" (Удаление) с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="1bf66-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="1bf66-258">Страница "Delete" (Удаление) обнаруживает конфликты параллелизма при изменении сущности после ее получения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="1bf66-259">`Department.RowVersion` является версией строки при получении сущности.</span><span class="sxs-lookup"><span data-stu-id="1bf66-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="1bf66-260">Когда EF Core создает команду SQL DELETE, она включает предложение WHERE с `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="1bf66-261">Если команда SQL DELETE не затрагивает ни одной строки:</span><span class="sxs-lookup"><span data-stu-id="1bf66-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="1bf66-262">`RowVersion` в команде SQL DELETE не соответствует `RowVersion` в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="1bf66-263">Возникает исключение DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="1bf66-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="1bf66-264">Вызывается `OnGetAsync` с `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="1bf66-265">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="1bf66-265">Update the Delete page</span></span>

<span data-ttu-id="1bf66-266">Измените *Pages/Departments/Delete.cshtml*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="1bf66-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="1bf66-267">Приведенная выше разметка вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="1bf66-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="1bf66-268">Изменяет директиву `page` с `@page` на `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1bf66-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="1bf66-269">Добавляет сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="1bf66-269">Adds an error message.</span></span>
* <span data-ttu-id="1bf66-270">Заменяет FirstMidName на FullName в поле **Administrator** (Администратор).</span><span class="sxs-lookup"><span data-stu-id="1bf66-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="1bf66-271">Изменяет `RowVersion` для отображения последнего байта.</span><span class="sxs-lookup"><span data-stu-id="1bf66-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="1bf66-272">Добавляет версию скрытых строк.</span><span class="sxs-lookup"><span data-stu-id="1bf66-272">Adds a hidden row version.</span></span> <span data-ttu-id="1bf66-273">Нужно добавить `RowVersion`, чтобы обратная передача привязывала значение.</span><span class="sxs-lookup"><span data-stu-id="1bf66-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="1bf66-274">Тестирование конфликтов параллелизма с использованием страницы "Delete" (Удаление)</span><span class="sxs-lookup"><span data-stu-id="1bf66-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="1bf66-275">Создайте тестовую кафедру.</span><span class="sxs-lookup"><span data-stu-id="1bf66-275">Create a test department.</span></span>

<span data-ttu-id="1bf66-276">Откройте в браузере два экземпляра страницы "Delete" (Удаление) для тестовой кафедры:</span><span class="sxs-lookup"><span data-stu-id="1bf66-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="1bf66-277">Запустите приложение и выберите "Departments" (Кафедры).</span><span class="sxs-lookup"><span data-stu-id="1bf66-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="1bf66-278">Щелкните правой кнопкой мыши гиперссылку **Delete** (Удалить) для тестовой кафедры и выберите пункт **Открыть на новой вкладке**.</span><span class="sxs-lookup"><span data-stu-id="1bf66-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="1bf66-279">Щелкните гиперссылку **Edit** (Изменить) для тестовой кафедры.</span><span class="sxs-lookup"><span data-stu-id="1bf66-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="1bf66-280">На обеих вкладках браузера отображаются одинаковые сведения.</span><span class="sxs-lookup"><span data-stu-id="1bf66-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="1bf66-281">Измените бюджет на первой вкладке браузера и нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="1bf66-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="1bf66-282">В браузере отображается страница индекса с измененным значением и обновленным индикатором rowVersion.</span><span class="sxs-lookup"><span data-stu-id="1bf66-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="1bf66-283">Обратите внимание на обновленный индикатор rowVersion, он отображается при второй обратной передаче на другой вкладке.</span><span class="sxs-lookup"><span data-stu-id="1bf66-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="1bf66-284">Удалите тестовую кафедру со второй закладки. Отображается ошибка параллелизма с текущими значениями из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1bf66-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="1bf66-285">При нажатии кнопки **Delete** (Удалить) сущность удаляется, если только не был обновлен `RowVersion` или не была удалена кафедра.</span><span class="sxs-lookup"><span data-stu-id="1bf66-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="1bf66-286">Сведения о том, как наследовать модель данных, см. в разделе [Наследование](xref:data/ef-mvc/inheritance).</span><span class="sxs-lookup"><span data-stu-id="1bf66-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1bf66-287">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1bf66-287">Additional resources</span></span>

* [<span data-ttu-id="1bf66-288">Токены параллелизма в EF Core</span><span class="sxs-lookup"><span data-stu-id="1bf66-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [<span data-ttu-id="1bf66-289">Обработка параллелизма в EF Core</span><span class="sxs-lookup"><span data-stu-id="1bf66-289">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="1bf66-290">Назад</span><span class="sxs-lookup"><span data-stu-id="1bf66-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
