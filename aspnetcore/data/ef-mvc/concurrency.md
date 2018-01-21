---
title: "Основные ASP.NET MVC с основными EF - параллелизма - 8, 10"
author: tdykstra
description: "Этот учебник показывает, как для обработки конфликтов, когда нескольким пользователям обновлять ту же сущность в то же время."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 69ffafc7f92cda75c001fe1098275766063113fb
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="ca18c-103">Обработка конфликтов параллелизма - Core EF учебнику ASP.NET Core MVC (8, 10)</span><span class="sxs-lookup"><span data-stu-id="ca18c-103">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="ca18c-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca18c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca18c-105">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca18c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="ca18c-106">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="ca18c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="ca18c-107">В предыдущих учебниках вы узнали, как обновлять данные.</span><span class="sxs-lookup"><span data-stu-id="ca18c-107">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="ca18c-108">Этот учебник показывает, как для обработки конфликтов, когда нескольким пользователям обновлять ту же сущность в то же время.</span><span class="sxs-lookup"><span data-stu-id="ca18c-108">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="ca18c-109">Вы создадите веб-страниц, которые работают с сущности «отдел» и обработки ошибок параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-109">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="ca18c-110">На следующих рисунках страницы Edit и Delete, включая некоторые сообщения, которые отображаются, если возникает конфликт параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-110">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Страница отдела изменения](concurrency/_static/edit-error.png)

![Страница отдела Delete](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="ca18c-113">Конфликты параллелизма</span><span class="sxs-lookup"><span data-stu-id="ca18c-113">Concurrency conflicts</span></span>

<span data-ttu-id="ca18c-114">Конфликт параллелизма возникает, когда один пользователь отображены данные сущности для его изменения, а затем другой пользователь обновляет данные той же сущности перед изменение первый пользователь будет записано в базу данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-114">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="ca18c-115">Если не включить обнаружение такие конфликты, кто обновляет базу данных, перезаписывает изменения другого пользователя.</span><span class="sxs-lookup"><span data-stu-id="ca18c-115">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="ca18c-116">Во многих приложениях приемлемо этот риск: при наличии нескольких пользователей или несколько обновлений, или если не действительно являются критическими, если некоторые изменения будут перезаписаны, стоимость программирования для параллелизма может перевесить преимущества.</span><span class="sxs-lookup"><span data-stu-id="ca18c-116">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="ca18c-117">В этом случае не нужно настроить приложение для обработки конфликтов параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-117">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="ca18c-118">Пессимистическая блокировка (блокировки)</span><span class="sxs-lookup"><span data-stu-id="ca18c-118">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="ca18c-119">Если приложению требуется предотвратить случайную потерю данных в сценарии параллелизма, один из способов сделать это — использовать блокировки базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-119">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="ca18c-120">Это называется пессимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-120">This is called pessimistic concurrency.</span></span> <span data-ttu-id="ca18c-121">Например, перед прочтением строки из базы данных запрашивается блокировка для только для чтения или для доступ для обновления.</span><span class="sxs-lookup"><span data-stu-id="ca18c-121">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="ca18c-122">При блокировке строку для обновления доступ другие пользователи не разрешены для блокировки строки, либо для только для чтения или обновления доступа, так как он может получить копию данных, которая находится в процессе изменения.</span><span class="sxs-lookup"><span data-stu-id="ca18c-122">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="ca18c-123">При блокировке строк для доступа только для чтения, другие также можно блокировать его для доступа только для чтения, но не для обновления.</span><span class="sxs-lookup"><span data-stu-id="ca18c-123">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="ca18c-124">Управление блокировками имеет недостатки.</span><span class="sxs-lookup"><span data-stu-id="ca18c-124">Managing locks has disadvantages.</span></span> <span data-ttu-id="ca18c-125">Он может оказаться непростой задачей программы.</span><span class="sxs-lookup"><span data-stu-id="ca18c-125">It can be complex to program.</span></span> <span data-ttu-id="ca18c-126">Она требует значительные ресурсы базы данных управления, и он может вызвать снижение производительности как число пользователей приложения увеличивается.</span><span class="sxs-lookup"><span data-stu-id="ca18c-126">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="ca18c-127">По этим причинам не всех систем управления базами данных поддерживают пессимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-127">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="ca18c-128">Entity Framework Core предоставляет нет встроенная поддержка и учебнике не показано, как для его реализации.</span><span class="sxs-lookup"><span data-stu-id="ca18c-128">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="ca18c-129">Оптимистическая блокировка</span><span class="sxs-lookup"><span data-stu-id="ca18c-129">Optimistic Concurrency</span></span>

<span data-ttu-id="ca18c-130">Альтернативой пессимистичный параллелизм является оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-130">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="ca18c-131">Оптимистический параллелизм означает разрешение конфликтов параллелизма активна и затем отклик соответствующим образом, если они есть.</span><span class="sxs-lookup"><span data-stu-id="ca18c-131">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="ca18c-132">Например Мария — страница отдела изменять и изменяет сумму бюджета для английского языка отдела с $350,000.00 на 0,00 долларов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-132">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Изменение бюджета 0](concurrency/_static/change-budget.png)

<span data-ttu-id="ca18c-134">Перед щелкает Мария **Сохранить**, Джон посещает той же странице и изменяет поле Дата начала 9/1/2007 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="ca18c-134">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Изменение даты начала 2013](concurrency/_static/change-date.png)

<span data-ttu-id="ca18c-136">Мария щелкает **Сохранить** первый и видит ее изменить, если браузер возвращается на страницу индекса.</span><span class="sxs-lookup"><span data-stu-id="ca18c-136">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Бюджет изменено на ноль](concurrency/_static/budget-zero.png)

<span data-ttu-id="ca18c-138">Затем Джон щелкает **Сохранить** на страницу редактирования, которая по-прежнему показывает бюджет 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="ca18c-138">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="ca18c-139">Дальнейший ход событий определяется порядок обработки конфликтов параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-139">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="ca18c-140">Ниже приведены некоторые из параметров:</span><span class="sxs-lookup"><span data-stu-id="ca18c-140">Some of the options include the following:</span></span>

* <span data-ttu-id="ca18c-141">Можно хранить список какие свойства были изменены пользователем и обновлять соответствующие столбцы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-141">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="ca18c-142">В примере сценария данные не будут потеряны, поскольку различные свойства были обновлены двумя пользователями.</span><span class="sxs-lookup"><span data-stu-id="ca18c-142">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="ca18c-143">При очередном кто-то просматривает отделе английского языка, они видят изменения Джейн и Джона--дату начала для 9/1/2013 и бюджет ноль долларов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-143">The next time someone browses the English department, they'll see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="ca18c-144">Этот метод обновления может снизить количество конфликтов, которые могут привести к потере данных, но его не удается избежать потери данных, если конкурирующих изменений в то же свойство сущности.</span><span class="sxs-lookup"><span data-stu-id="ca18c-144">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="ca18c-145">Работает ли платформа Entity Framework следующим образом зависит от того, как реализовать код обновления.</span><span class="sxs-lookup"><span data-stu-id="ca18c-145">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="ca18c-146">Нецелесообразно часто веб-приложения, так как он может потребоваться поддерживать большое количество состояний, чтобы отслеживать все исходные значения свойств для сущности, а также новые значения.</span><span class="sxs-lookup"><span data-stu-id="ca18c-146">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="ca18c-147">Обслуживание больших объемов состояния может повлиять на производительность приложения, так как он требует ресурсы сервера или должны быть включены на странице веб-(например, в скрытых полях) или в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="ca18c-147">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="ca18c-148">Можно разрешить изменение Джона перезаписать Джейн изменений.</span><span class="sxs-lookup"><span data-stu-id="ca18c-148">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="ca18c-149">Далее время кто-то просматривает отделе английского языка, они видят 9/1/2013 и восстановленное значение $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="ca18c-149">The next time someone browses the English department, they'll see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="ca18c-150">Это называется *клиент побеждает* или *побеждает последний* сценария.</span><span class="sxs-lookup"><span data-stu-id="ca18c-150">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="ca18c-151">(Все значения из клиента имеют приоритет над возможности хранилища данных). Как отмечено во введении к этому разделу, если этого не сделать, написания кода для обработки параллелизма, она выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="ca18c-151">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="ca18c-152">Можно запретить изменение Джона обновляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-152">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="ca18c-153">Как правило следует отобразить сообщение об ошибке, отображения его текущего состояния данных и позволяют ему для повторного применения изменений, если он по-прежнему хочет сделать их.</span><span class="sxs-lookup"><span data-stu-id="ca18c-153">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="ca18c-154">Это называется *побеждает хранилища* сценария.</span><span class="sxs-lookup"><span data-stu-id="ca18c-154">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="ca18c-155">(Значения хранилища данных имеют приоритет над значениями, отправленные клиентом). В этом учебнике необходимо реализовать сценарий Wins хранилища.</span><span class="sxs-lookup"><span data-stu-id="ca18c-155">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="ca18c-156">Этот метод гарантирует, что изменения не переписываются без пользователя предупреждения о происходящем.</span><span class="sxs-lookup"><span data-stu-id="ca18c-156">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="ca18c-157">Обнаружение конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="ca18c-157">Detecting concurrency conflicts</span></span>

<span data-ttu-id="ca18c-158">Конфликты можно разрешать путем обработки `DbConcurrencyException` исключения, которые выдает Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ca18c-158">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="ca18c-159">Чтобы определить, когда необходимо создавать эти исключения, Entity Framework необходима возможность обнаружения конфликтов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-159">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="ca18c-160">Таким образом необходимо настроить базу данных и модели данных соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="ca18c-160">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="ca18c-161">Ниже приведены некоторые параметры для включения обнаружения конфликтов:</span><span class="sxs-lookup"><span data-stu-id="ca18c-161">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="ca18c-162">В таблице базы данных включают столбец отслеживания, который может использоваться для определения момента изменения строки.</span><span class="sxs-lookup"><span data-stu-id="ca18c-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="ca18c-163">Затем можно настроить Entity Framework, чтобы включить этот столбец в предложении Where инструкции SQL Update или Delete команд.</span><span class="sxs-lookup"><span data-stu-id="ca18c-163">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="ca18c-164">Тип данных столбца отслеживания обычно является `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="ca18c-164">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="ca18c-165">`rowversion` Значение — это число, увеличивающееся каждый раз при обновлении строки.</span><span class="sxs-lookup"><span data-stu-id="ca18c-165">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="ca18c-166">В команде Update или Delete предложение Where, предложение содержит исходное значение столбца отслеживания (версии).</span><span class="sxs-lookup"><span data-stu-id="ca18c-166">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="ca18c-167">Если были изменены другим пользователем, значение в обновляемой строке `rowversion` столбца отличается от исходное значение, поэтому инструкции Update или Delete не удается найти строки обновления из-за Where предложения.</span><span class="sxs-lookup"><span data-stu-id="ca18c-167">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="ca18c-168">Когда команды Entity Framework обнаруживает, что строки не были обновлены, Update или Delete (то есть, когда количество задействованных строк равно нулю), он интерпретирует как конфликт параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-168">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="ca18c-169">Настройка Entity Framework для включения исходных значений для каждого из столбцов в таблице в предложении Where инструкции команды Update и Delete.</span><span class="sxs-lookup"><span data-stu-id="ca18c-169">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="ca18c-170">Как и первый вариант, если что-либо в строке изменилась с момента прочтите строки где предложение не возвращает строки для обновления, который Entity Framework интерпретирует как конфликт параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-170">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="ca18c-171">Для таблиц базы данных, имеющие много столбцов, этот подход может привести к очень больших Where предложений и может потребоваться поддерживать большие объемы состояния.</span><span class="sxs-lookup"><span data-stu-id="ca18c-171">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="ca18c-172">Как отмечалось ранее, обслуживание больших объемов состояния может повлиять на производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="ca18c-172">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="ca18c-173">Поэтому этот подход не рекомендуется и не метод, используемый в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="ca18c-173">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="ca18c-174">Если необходимо реализовать этот подход к параллелизма, необходимо пометить все свойства первичного ключа в сущность, которую необходимо отслеживать параллелизм для добавляя `ConcurrencyCheck` к ним атрибут.</span><span class="sxs-lookup"><span data-stu-id="ca18c-174">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="ca18c-175">Это изменение позволяет платформе Entity Framework выбрать все столбцы в предложении SQL Where инструкции Update и Delete.</span><span class="sxs-lookup"><span data-stu-id="ca18c-175">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="ca18c-176">В оставшейся части этого учебника вам предстоит добавить `rowversion` отслеживание свойства сущности «отдел», создать контроллер и представления и проверить, что все работает правильно.</span><span class="sxs-lookup"><span data-stu-id="ca18c-176">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="ca18c-177">Добавление свойства отслеживания в сущности «отдел»</span><span class="sxs-lookup"><span data-stu-id="ca18c-177">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="ca18c-178">В *Models/Department.cs*, добавьте RowVersion следящее свойство:</span><span class="sxs-lookup"><span data-stu-id="ca18c-178">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="ca18c-179">`Timestamp` Атрибут указывает, что этот столбец будет включен в предложении Where инструкции команды Update и Delete, отправляемые в базу данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-179">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="ca18c-180">Атрибут называется `Timestamp` из-за предыдущих версий SQL Server используется SQL `timestamp` типа данных перед SQL `rowversion` заменил его.</span><span class="sxs-lookup"><span data-stu-id="ca18c-180">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="ca18c-181">Тип .NET для `rowversion` — массив байтов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-181">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="ca18c-182">Если вы предпочитаете использовать fluent API, можно использовать `IsConcurrencyToken` метода (в *Data/SchoolContext.cs*) для указания свойства отслеживания, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ca18c-182">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="ca18c-183">При добавлении свойства изменить модель базы данных, необходимо выполнить еще один процесс миграции.</span><span class="sxs-lookup"><span data-stu-id="ca18c-183">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="ca18c-184">Сохранить изменения, постройте проект и введите следующие команды в командную строку:</span><span class="sxs-lookup"><span data-stu-id="ca18c-184">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="ca18c-185">Создание подразделения контроллеров и представления</span><span class="sxs-lookup"><span data-stu-id="ca18c-185">Create a Departments controller and views</span></span>

<span data-ttu-id="ca18c-186">Формировать контроллер отделов и представления, как это было ранее для учащихся, курсы и инструкторов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-186">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Отдел формирования шаблонов](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="ca18c-188">В *DepartmentsController.cs* файл, изменить все четыре вхождения «FirstMidName» на «Полное имя», чтобы раскрывающиеся списки администратор отдела будет содержать полное имя инструктора, а не просто фамилию.</span><span class="sxs-lookup"><span data-stu-id="ca18c-188">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="ca18c-189">Обновить представление индекса отделов</span><span class="sxs-lookup"><span data-stu-id="ca18c-189">Update the Departments Index view</span></span>

<span data-ttu-id="ca18c-190">Механизм формирования шаблонов в представлении индекса создается столбец RowVersion, но это поле не должно отображаться.</span><span class="sxs-lookup"><span data-stu-id="ca18c-190">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="ca18c-191">Замените код в *Views/Departments/Index.cshtml* следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="ca18c-191">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="ca18c-192">Это изменяет заголовок «Отделы» удаляет столбец RowVersion и показано полное имя, а не имя администратора.</span><span class="sxs-lookup"><span data-stu-id="ca18c-192">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="ca18c-193">Методы изменения в контроллере отделов обновления</span><span class="sxs-lookup"><span data-stu-id="ca18c-193">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="ca18c-194">В обоих HttpGet `Edit` метод и `Details` метод, добавьте `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="ca18c-194">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="ca18c-195">В HttpGet `Edit` метод, добавьте упреждающую администратора.</span><span class="sxs-lookup"><span data-stu-id="ca18c-195">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="ca18c-196">Замените существующий код для HttpPost `Edit` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ca18c-196">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="ca18c-197">Код начинает работу с чтения отдел обновляться.</span><span class="sxs-lookup"><span data-stu-id="ca18c-197">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="ca18c-198">Если `SingleOrDefaultAsync` метод возвращает значение null, отдел был удален другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="ca18c-198">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="ca18c-199">В этом случае код использует значения из отправленной формы для создания сущности отдела, чтобы страницы правки можно повторно с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="ca18c-199">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="ca18c-200">В качестве альтернативы не нужно повторно создать сущности «отдел», если отображается сообщение об ошибке без повторное отображение поля отдел.</span><span class="sxs-lookup"><span data-stu-id="ca18c-200">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="ca18c-201">Представление сохраняет исходное `RowVersion` значения в скрытом поле и этот метод получает это значение в `rowVersion` параметра.</span><span class="sxs-lookup"><span data-stu-id="ca18c-201">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="ca18c-202">Перед вызовом метода `SaveChanges`, необходимо установить, исходное `RowVersion` значение свойства в `OriginalValues` коллекции для сущности.</span><span class="sxs-lookup"><span data-stu-id="ca18c-202">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="ca18c-203">Затем платформа Entity Framework создает команду SQL UPDATE, эта команда включает предложения WHERE, которое ищет строку, которая содержит исходный `RowVersion` значение.</span><span class="sxs-lookup"><span data-stu-id="ca18c-203">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="ca18c-204">Если нет строк, затронутых командой обновления (не строки имеют исходное `RowVersion` значение), Entity Framework создает исключение `DbUpdateConcurrencyException` исключение.</span><span class="sxs-lookup"><span data-stu-id="ca18c-204">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="ca18c-205">Код в блоке catch для этого исключения возвращает затронутые сущность Department, обновленные значения из `Entries` свойства объекта исключения.</span><span class="sxs-lookup"><span data-stu-id="ca18c-205">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="ca18c-206">`Entries` Коллекция будет содержать только один `EntityEntry` объекта.</span><span class="sxs-lookup"><span data-stu-id="ca18c-206">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="ca18c-207">Этот объект можно использовать для получения новых значений, введенных пользователем и текущие значения базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-207">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="ca18c-208">Код добавляет сообщения о пользовательской ошибке для каждого столбца, имеющего различные значения базы данных, из какой введенные пользователем на редактирование страницы (только одно поле, приведен здесь для краткости).</span><span class="sxs-lookup"><span data-stu-id="ca18c-208">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="ca18c-209">Наконец, код задает `RowVersion` значение `departmentToUpdate` новое значение, извлеченные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-209">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="ca18c-210">Этот новый `RowVersion` значение будет храниться в скрытое поле, когда изменение, снова откроется страница и следующий раз пользователь щелкает **Сохранить**, только ошибки параллелизма, которые происходят с момента будет порождено повторно отобразить страницы правки.</span><span class="sxs-lookup"><span data-stu-id="ca18c-210">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="ca18c-211">`ModelState.Remove` Инструкция является обязательным, поскольку `ModelState` имеет старый `RowVersion` значение.</span><span class="sxs-lookup"><span data-stu-id="ca18c-211">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="ca18c-212">В представлении `ModelState` значение для поля имеет приоритет над значения свойств модели, если установлены оба.</span><span class="sxs-lookup"><span data-stu-id="ca18c-212">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="ca18c-213">Обновить представление изменить подразделение</span><span class="sxs-lookup"><span data-stu-id="ca18c-213">Update the Department Edit view</span></span>

<span data-ttu-id="ca18c-214">В *Views/Departments/Edit.cshtml*, внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="ca18c-214">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="ca18c-215">Добавление скрытого поля для сохранения `RowVersion` значение свойства, следующий сразу за скрытого поля для `DepartmentID` свойства.</span><span class="sxs-lookup"><span data-stu-id="ca18c-215">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="ca18c-216">Добавьте пункт «Выберите Administrator» в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="ca18c-216">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="ca18c-217">Тестирование конфликтов параллелизма в страницы правки</span><span class="sxs-lookup"><span data-stu-id="ca18c-217">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="ca18c-218">Запустите приложение и перейти на страницу индекса отделов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-218">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="ca18c-219">Щелкните правой кнопкой мыши **изменить** гиперссылку для английского языка подразделение и выберите **открыть в новой вкладке**, нажмите кнопку **изменить** гиперссылки для английского языка отдела.</span><span class="sxs-lookup"><span data-stu-id="ca18c-219">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="ca18c-220">Вкладки два браузера теперь отображаются те же сведения.</span><span class="sxs-lookup"><span data-stu-id="ca18c-220">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="ca18c-221">Измените поле на первой вкладке браузера и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="ca18c-221">Change a field in the first browser tab and click **Save**.</span></span>

![Изменение подразделения страница 1 после изменения](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="ca18c-223">В браузере будет отображена страница индекса с измененное значение.</span><span class="sxs-lookup"><span data-stu-id="ca18c-223">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="ca18c-224">Изменение поля на второй вкладке браузера.</span><span class="sxs-lookup"><span data-stu-id="ca18c-224">Change a field in the second browser tab.</span></span>

![Изменение подразделения страница 2 после изменения](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="ca18c-226">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="ca18c-226">Click **Save**.</span></span> <span data-ttu-id="ca18c-227">Вы видите сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="ca18c-227">You see an error message:</span></span>

![Сообщение об ошибке отдел изменить страницу](concurrency/_static/edit-error.png)

<span data-ttu-id="ca18c-229">Нажмите кнопку **Сохранить** еще раз.</span><span class="sxs-lookup"><span data-stu-id="ca18c-229">Click **Save** again.</span></span> <span data-ttu-id="ca18c-230">Сохраняется значение, введенное на второй вкладке браузера.</span><span class="sxs-lookup"><span data-stu-id="ca18c-230">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="ca18c-231">Появиться сохраненные значения при появлении страницы индекса.</span><span class="sxs-lookup"><span data-stu-id="ca18c-231">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="ca18c-232">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="ca18c-232">Update the Delete page</span></span>

<span data-ttu-id="ca18c-233">На странице Delete Entity Framework обнаруживает конфликты параллелизма, причиной else редактирования отделе так же, как.</span><span class="sxs-lookup"><span data-stu-id="ca18c-233">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="ca18c-234">Когда HttpGet `Delete` метод отображает представление подтверждения, представление включает в себя исходный `RowVersion` значения в скрытом поле.</span><span class="sxs-lookup"><span data-stu-id="ca18c-234">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="ca18c-235">Что значения затем становится доступным для HttpPost `Delete` метод, который вызывается, когда пользователь подтверждения удаления.</span><span class="sxs-lookup"><span data-stu-id="ca18c-235">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="ca18c-236">Когда платформа Entity Framework создает команду SQL DELETE, он включает предложение WHERE с первоначальным `RowVersion` значение.</span><span class="sxs-lookup"><span data-stu-id="ca18c-236">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="ca18c-237">Если результаты команды в ноль строк влияет (то есть строка была изменена после отображается страница подтверждения удаления), исключение параллелизма и HttpGet `Delete` метод вызывается с установленным флагом ошибка значение true, чтобы снова отобразить страница подтверждения с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="ca18c-237">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="ca18c-238">Это также возможно, что нулевые затронуты, так как строка была удалена другим пользователем, поэтому в этом случае сообщение об ошибке не отображается.</span><span class="sxs-lookup"><span data-stu-id="ca18c-238">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="ca18c-239">Методы удаления в контроллере отделов обновления</span><span class="sxs-lookup"><span data-stu-id="ca18c-239">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="ca18c-240">В *DepartmentsController.cs*, замените HttpGet `Delete` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ca18c-240">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="ca18c-241">Этот метод принимает необязательный параметр, который указывает, выполняется ли страница отобразится после ошибки параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-241">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="ca18c-242">Если этот флаг имеет значение true и отдел, больше не существует, он был удален другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="ca18c-242">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="ca18c-243">В этом случае код выполняет перенаправление на страницу индекса.</span><span class="sxs-lookup"><span data-stu-id="ca18c-243">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="ca18c-244">Если этот флаг имеет значение true, а отдел существует, он был изменен другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="ca18c-244">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="ca18c-245">В этом случае код отправляет сообщение об ошибке представление, используя `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ca18c-245">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="ca18c-246">Замените код в HttpPost `Delete` метод (с именем `DeleteConfirmed`) следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ca18c-246">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="ca18c-247">В создании кода, который Вы заменили этот метод принято только идентификатор записи:</span><span class="sxs-lookup"><span data-stu-id="ca18c-247">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="ca18c-248">Этот параметр был изменен для отдела экземпляр сущности, созданные связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="ca18c-248">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="ca18c-249">Это дает EF доступ к значению свойства RowVersion помимо ключа записи.</span><span class="sxs-lookup"><span data-stu-id="ca18c-249">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="ca18c-250">Изменились имя метода действия из `DeleteConfirmed` для `Delete`.</span><span class="sxs-lookup"><span data-stu-id="ca18c-250">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="ca18c-251">Код формирования шаблонов используется имя `DeleteConfirmed` для предоставления метода HttpPost уникальная сигнатура.</span><span class="sxs-lookup"><span data-stu-id="ca18c-251">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="ca18c-252">(Среда CLR требуется перегруженные методы, чтобы иметь параметры другой метод). Теперь, когда сигнатуры уникальны, можно не покидайте соглашение MVC и использовать то же имя для удаления методов HttpPost и HttpGet.</span><span class="sxs-lookup"><span data-stu-id="ca18c-252">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="ca18c-253">Если подразделение уже удален, `AnyAsync` метод возвращает значение false, и приложение просто возвратится к методу Index.</span><span class="sxs-lookup"><span data-stu-id="ca18c-253">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="ca18c-254">При одновременном доступе, код заново отобразит страницу подтверждения удаления и предоставляет флаг, указывающий, он должен отображать сообщение об ошибке с параллелизмом.</span><span class="sxs-lookup"><span data-stu-id="ca18c-254">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="ca18c-255">Обновление представления удаления</span><span class="sxs-lookup"><span data-stu-id="ca18c-255">Update the Delete view</span></span>

<span data-ttu-id="ca18c-256">В *Views/Departments/Delete.cshtml*, замените формирования шаблонов код следующим кодом, который добавляет поля ошибки сообщения и скрытые поля свойств DepartmentID и RowVersion.</span><span class="sxs-lookup"><span data-stu-id="ca18c-256">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="ca18c-257">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="ca18c-257">The changes are highlighted.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="ca18c-258">Это вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="ca18c-258">This makes the following changes:</span></span>

* <span data-ttu-id="ca18c-259">Добавляет сообщение об ошибке между `h2` и `h3` заголовков.</span><span class="sxs-lookup"><span data-stu-id="ca18c-259">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="ca18c-260">Заменяет FirstMidName FullName в **администратора** поля.</span><span class="sxs-lookup"><span data-stu-id="ca18c-260">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="ca18c-261">Удаляет поле RowVersion.</span><span class="sxs-lookup"><span data-stu-id="ca18c-261">Removes the RowVersion field.</span></span>

* <span data-ttu-id="ca18c-262">Добавляет скрытое поле для `RowVersion` свойства.</span><span class="sxs-lookup"><span data-stu-id="ca18c-262">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="ca18c-263">Запустите приложение и перейти на страницу индекса отделов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-263">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="ca18c-264">Щелкните правой кнопкой мыши **удалить** гиперссылку для английского языка подразделение и выберите **открыть в новой вкладке**, выберите в первой позицией табуляции **изменить** гиперссылки для английского языка отдела.</span><span class="sxs-lookup"><span data-stu-id="ca18c-264">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="ca18c-265">В первом окне, измените одно из значений и нажмите кнопку **Сохранить**:</span><span class="sxs-lookup"><span data-stu-id="ca18c-265">In the first window, change one of the values, and click **Save**:</span></span>

![Страница отдела редактирования после изменения, прежде чем удалить](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="ca18c-267">На второй вкладке щелкните **удалить**.</span><span class="sxs-lookup"><span data-stu-id="ca18c-267">In the second tab, click **Delete**.</span></span> <span data-ttu-id="ca18c-268">Вы видите сообщение об ошибке с параллелизмом, а отдел значения обновляются с возможности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca18c-268">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Страница подтверждения удаления отдел с ошибкой параллелизма](concurrency/_static/delete-error.png)

<span data-ttu-id="ca18c-270">Если щелкнуть **удалить** еще раз, будет перенаправлен на страницу индекса, которая показывает, отдел был удален.</span><span class="sxs-lookup"><span data-stu-id="ca18c-270">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="ca18c-271">Дополнительные сведения об обновлении и создавать представления</span><span class="sxs-lookup"><span data-stu-id="ca18c-271">Update Details and Create views</span></span>

<span data-ttu-id="ca18c-272">Можно при необходимости очистить формирования шаблонов кода, в области сведений и создания представлений.</span><span class="sxs-lookup"><span data-stu-id="ca18c-272">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="ca18c-273">Замените код в *Views/Departments/Details.cshtml* удалить столбец RowVersion и Показывать полное имя администратора.</span><span class="sxs-lookup"><span data-stu-id="ca18c-273">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="ca18c-274">Замените код в *Views/Departments/Create.cshtml* Добавление выберите параметр в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="ca18c-274">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="ca18c-275">Сводка</span><span class="sxs-lookup"><span data-stu-id="ca18c-275">Summary</span></span>

<span data-ttu-id="ca18c-276">На этом завершается Общие сведения для обработки конфликтов параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ca18c-276">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="ca18c-277">Дополнительные сведения о способах обработки параллелизма в ядре EF в разделе [конфликтов параллелизма](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="ca18c-277">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="ca18c-278">Далее учебнике демонстрируется реализация таблица на иерархию наследования для сущности инструктора и студентов.</span><span class="sxs-lookup"><span data-stu-id="ca18c-278">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ca18c-279">[Назад](update-related-data.md)
[Вперед](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="ca18c-279">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
