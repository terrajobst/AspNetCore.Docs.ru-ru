---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Обработка параллелизма с платформой Entity Framework 6 в приложении ASP.NET MVC 5 (10, 12) | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e7b79503a1d297d40c6824f8b2b7bbc1f42b9fca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Обработка параллелизма с платформой Entity Framework 6 в приложении ASP.NET MVC 5 (10, 12)
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущих учебниках вы узнали, как обновлять данные. Этот учебник показывает, как для обработки конфликтов, когда нескольким пользователям обновлять ту же сущность в то же время.

Вы измените веб-страниц, которые работают с `Department` сущности, чтобы они обрабатывают ошибок параллелизма. На следующих рисунках страницы индекса и Delete, включая некоторые сообщения, которые отображаются, если возникает конфликт параллелизма.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь отображены данные сущности для его изменения, а затем другой пользователь обновляет данные той же сущности перед изменение первый пользователь будет записано в базу данных. Если не включить обнаружение такие конфликты, кто обновляет базу данных, перезаписывает изменения другого пользователя. Во многих приложениях приемлемо этот риск: при наличии нескольких пользователей или несколько обновлений, или если не действительно являются критическими, если некоторые изменения будут перезаписаны, стоимость программирования для параллелизма может перевесить преимущества. В этом случае не нужно настроить приложение для обработки конфликтов параллелизма.

### <a name="pessimistic-concurrency-locking"></a>Пессимистическая блокировка (блокировки)

Если приложению требуется предотвратить случайную потерю данных в сценарии параллелизма, один из способов сделать это — использовать блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед прочтением строки из базы данных запрашивается блокировка для только для чтения или для доступ для обновления. При блокировке строку для обновления доступ другие пользователи не разрешены для блокировки строки, либо для только для чтения или обновления доступа, так как он может получить копию данных, которая находится в процессе изменения. При блокировке строк для доступа только для чтения, другие также можно блокировать его для доступа только для чтения, но не для обновления.

Управление блокировками имеет недостатки. Он может оказаться непростой задачей программы. Она требует значительные ресурсы базы данных управления, и он может вызвать снижение производительности как число пользователей приложения увеличивается. По этим причинам не всех систем управления базами данных поддерживают пессимистичного параллелизма. Платформа Entity Framework предоставляет нет встроенная поддержка и учебнике не показано, как для его реализации.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Вместо него следует использовать для пессимистичный параллелизм *оптимистичного параллелизма*. Оптимистический параллелизм означает разрешение конфликтов параллелизма активна и затем отклик соответствующим образом, если они есть. Например, Джон запускает отделов изменить страницу, изменения **бюджета** сумма для английского языка отдела из 350,000.00 $ $ 0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Прежде чем Джон выбирает **Сохранить**, Мария запускает одну и ту же страницу и изменения **Дата начала** поля из 9/1/2007 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Джон выбирает **Сохранить** первым и его изменение браузер по возвращении на страницу индекса, а затем Мария щелкает видит **Сохранить**. Дальнейший ход событий определяется порядок обработки конфликтов параллелизма. Ниже приведены некоторые из параметров:

- Можно хранить список какие свойства были изменены пользователем и обновлять соответствующие столбцы в базе данных. В примере сценария данные не будут потеряны, поскольку различные свойства были обновлены двумя пользователями. Далее время кто-то просматривает отделе английского языка, они видят изменения Джон и Джейн — Начальная дата 8/8/2013 и бюджета ноль долларов.

    Этот метод обновления может снизить количество конфликтов, которые могут привести к потере данных, но его не удается избежать потери данных, если конкурирующих изменений в то же свойство сущности. Работает ли платформа Entity Framework следующим образом зависит от того, как реализовать код обновления. Нецелесообразно часто веб-приложения, так как он может потребоваться поддерживать большое количество состояний, чтобы отслеживать все исходные значения свойств для сущности, а также новые значения. Обслуживание больших объемов состояния может повлиять на производительность приложения, так как он требует ресурсы сервера или должны быть включены на странице веб-(например, в скрытых полях) или в файле cookie.
- Можно перезаписать изменения Джона изменение Джейн. Далее время кто-то просматривает отделе английского языка, они видят 8/8/2013 и восстановленное значение $350,000.00. Это называется *клиент побеждает* или *побеждает последний* сценария. (Все значения из клиента имеют приоритет над возможности хранилища данных). Как отмечено во введении к этому разделу, если этого не сделать, написания кода для обработки параллелизма, она выполняется автоматически.
- Можно запретить изменение Джейн обновляется в базе данных. Обычно сообщение об ошибке, Показать его текущее состояние данных и разрешить его для повторного применения своих изменений, если она по-прежнему хочет сделать их. Это называется *побеждает хранилища* сценария. (Значения хранилища данных имеют приоритет над значениями, отправленные клиентом). В этом учебнике необходимо реализовать сценарий Wins хранилища. Этот метод гарантирует, что изменения не переписываются без пользователя предупреждения о происходящем.

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

Конфликты можно разрешать путем обработки [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) исключения, которые выдает Entity Framework. Чтобы определить, когда необходимо создавать эти исключения, Entity Framework необходима возможность обнаружения конфликтов. Таким образом необходимо настроить базу данных и модели данных соответствующим образом. Ниже приведены некоторые параметры для включения обнаружения конфликтов:

- В таблице базы данных включают столбец отслеживания, который может использоваться для определения момента изменения строки. Затем можно настроить для включения этого столбца в Entity Framework `Where` предложение SQL `Update` или `Delete` команд.

    Тип данных столбца отслеживания обычно является [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) значение — это число, увеличивающееся каждый раз при обновлении строки. В `Update` или `Delete` команды `Where` предложение содержит исходное значение столбца отслеживания (версии). Если обновляемой строке был изменен другим пользователем, значение в `rowversion` столбца отличается от исходного значения, поэтому `Update` или `Delete` инструкции не удается найти строку для обновления из-за `Where` предложения. Когда Entity Framework находит, что строки не были обновлены с `Update` или `Delete` команды (то есть, когда количество задействованных строк равно нулю), он интерпретирует как конфликт параллелизма.
- Настройка включения исходных значений для каждого из столбцов в таблице в платформе Entity Framework `Where` предложения `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилась с момента прочтите строки `Where` предложение не возвращает строки для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Для таблиц базы данных, имеющие много столбцов, этот подход может привести к очень больших `Where` предложений и может потребоваться поддерживать большие объемы состояния. Как отмечалось ранее, обслуживание больших объемов состояния может повлиять на производительность приложения. Поэтому этот подход не рекомендуется и не метод, используемый в этом учебнике.

    Если необходимо реализовать этот подход к параллелизма, необходимо пометить все свойства первичного ключа в сущность, которую необходимо отслеживать параллелизм для добавляя [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) к ним атрибут. То, что изменение позволяет платформе Entity Framework включают все столбцы в инструкции SQL, `WHERE` предложения `UPDATE` инструкции.

В оставшейся части этого учебника вам предстоит добавить [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) свойство для отслеживания `Department` сущности, создать контроллер и представления и проверить, что все работает правильно.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Добавить свойство оптимистичного параллелизма для сущности «отдел»

В *Models\Department.cs*, добавьте отслеживания свойство с именем `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) атрибут указывает, что этот столбец будет включен в `Where` предложения `Update` и `Delete` команды, отправляемые в базу данных. Атрибут называется [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) из-за предыдущих версий SQL Server используется SQL [timestamp](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) типа данных перед SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) заменил его. Тип .net для [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) — массив байтов.

Если вы предпочитаете использовать fluent API, можно использовать [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) метод, чтобы задать свойства трассировки, как показано в следующем примере:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

При добавлении свойства изменить модель базы данных, необходимо выполнить еще один процесс миграции. В пакет Диспетчер консоли (PMC), введите следующие команды:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Изменение контроллера отдела

В *Controllers\DepartmentController.cs*, добавьте `using` инструкции:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

В *DepartmentController.cs* файл, изменить все четыре вхождения «LastName» на «Полное имя», чтобы раскрывающиеся списки администратор отдела будет содержать полное имя инструктора, а не просто фамилию.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Замените существующий код для `HttpPost` `Edit` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Если `FindAsync` метод возвращает значение null, отдел был удален другим пользователем. Код, показанный использует значения из отправленной формы для создания сущности отдела, чтобы страницы правки можно повторно с сообщением об ошибке. В качестве альтернативы не нужно повторно создать сущности «отдел», если отображается сообщение об ошибке без повторное отображение поля отдел.

Представление сохраняет исходное `RowVersion` значения в скрытом поле и метод получит его в `rowVersion` параметра. Перед вызовом метода `SaveChanges`, необходимо установить, исходное `RowVersion` значение свойства в `OriginalValues` коллекции для сущности. Затем в том случае, когда платформа Entity Framework создает SQL `UPDATE` команды, команда будет включать `WHERE` предложение, которое ищет строку, которая содержит исходный `RowVersion` значение.

Если строки не затронуты `UPDATE` команда (строки не имеют исходной `RowVersion` значение), Entity Framework создает исключение `DbUpdateConcurrencyException` исключения, а код в `catch` блок получает соответствующие `Department` сущности из исключения объект.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Этот объект содержит новые значения, введенные пользователем в его `Entity` свойство, чтобы получить значения, считанные из базы данных, вызвав `GetDatabaseValues` метод.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues` Метод возвращает значение null, если кто-то удалил строку из базы данных; в противном случае необходимо привести возвращенный объект к `Department` класса, чтобы получить доступ к `Department` свойства. (Так как уже установлен для удаления, `databaseEntry` будет иметь значение null, только если отдел был удален после `FindAsync` выполняет и перед `SaveChanges` выполняет.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Затем код добавляет сообщения о пользовательской ошибке для каждого столбца, имеющего значений базы данных, отличающийся от введенного пользователем на странице «Изменение»:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Длинное сообщение об ошибке объясняется, что произошло и что делать о нем.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Наконец, код задает `RowVersion` значение `Department` извлечь объект новое значение из базы данных. Этот новый `RowVersion` значение будет храниться в скрытое поле, когда изменение, снова откроется страница и следующий раз пользователь щелкает **Сохранить**, только ошибки параллелизма, которые происходят с момента будет порождено повторно отобразить страницы правки.

В *Views\Department\Edit.cshtml*, добавьте скрытое поле, чтобы сохранить `RowVersion` значение свойства, следующий сразу за скрытого поля для `DepartmentID` свойства:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Тестирование обработки оптимистичного параллелизма

Запустите сайт и нажмите кнопку **отделы**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Щелкните правой кнопкой мыши **изменить** гиперссылку для английского языка подразделение и выберите **открыть в новой вкладке** щелкните **изменить** гиперссылки для английского языка отдела. Две вкладки отображения той же информации.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Измените поле на первой вкладке браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

В браузере будет отображена страница индекса с измененное значение.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Измените поле на второй вкладке браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Нажмите кнопку **Сохранить** второй вкладке браузера. Вы видите сообщение об ошибке:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Нажмите кнопку **Сохранить** еще раз. Значение, введенное на второй вкладке браузера сохраняется вместе с исходное значение данных, измененные в первом браузера. Появиться сохраненные значения при появлении страницы индекса.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы удаления

На странице Delete Entity Framework обнаруживает конфликты параллелизма, причиной else редактирования отделе так же, как. Когда `HttpGet` `Delete` метод отображает представление подтверждения, представление включает в себя исходный `RowVersion` значения в скрытом поле. Значение затем становится доступным `HttpPost` `Delete` метод, который вызывается, когда пользователь подтверждения удаления. Если Entity Framework создает SQL `DELETE` команды, он включает `WHERE` предложение с первоначальным `RowVersion` значение. Если результаты команды в ноль строк влияет (то есть строка была изменена после отображается страница подтверждения удаления), исключение параллелизма и `HttpGet Delete` метод вызывается с ошибка установлен флаг `true` для повторного отображения страница подтверждения с сообщением об ошибке. Также возможно, что нулевые затронуты, так как строка была удалена другим пользователем, поэтому в этом случае отображается другое сообщение об ошибке.

В *DepartmentController.cs*, замените `HttpGet` `Delete` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Этот метод принимает необязательный параметр, который указывает, выполняется ли страница отобразится после ошибки параллелизма. Если этот флаг `true`, сообщение об ошибке отправляется в представлении с помощью `ViewBag` свойство.

Замените код в `HttpPost` `Delete` метод (с именем `DeleteConfirmed`) следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

В создании кода, который Вы заменили этот метод принято только идентификатор записи:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Вы изменили этот параметр, чтобы `Department` экземпляр сущности, созданные связывателя модели. Это дает доступ к `RowVersion` значение свойства помимо ключа записи.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Изменились имя метода действия из `DeleteConfirmed` для `Delete`. Формирования шаблонов код, называемый `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` метод уникальная сигнатура. (Среда CLR требуется перегруженные методы, чтобы иметь параметры другой метод). Теперь, когда сигнатурах являются уникальными, можно не покидайте соглашение MVC и использовать то же имя для `HttpPost` и `HttpGet` удаления методов.

При одновременном доступе, код заново отобразит страницу подтверждения удаления и предоставляет флаг, указывающий, он должен отображать сообщение об ошибке с параллелизмом.

В *Views\Department\Delete.cshtml*, замените формирования шаблонов код следующим кодом, который добавляет поля ошибки сообщения и скрытые поля свойств DepartmentID и RowVersion. Изменения выделены.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Этот код добавляет сообщение об ошибке между `h2` и `h3` заголовки:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Он заменяет `LastName` с `FullName` в `Administrator` поля:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Наконец, он добавляет скрытые поля для `DepartmentID` и `RowVersion` свойства после `Html.BeginForm` инструкции:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Запустите страницу индекса отделов. Щелкните правой кнопкой мыши **удаление** гиперссылку для английского языка подразделение и выберите **открыть в новой вкладке** на первой вкладке нажмите кнопку **изменить** гиперссылки для английского языка отдела.

В первом окне, измените одно из значений и нажмите кнопку **Сохранить** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Страница индекса подтверждает изменения.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

На второй вкладке щелкните **удалить**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Вы видите сообщение об ошибке с параллелизмом, а отдел значения обновляются с возможности в базе данных.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Если щелкнуть **удалить** еще раз, будет перенаправлен на страницу индекса, которая показывает, отдел был удален.

## <a name="summary"></a>Сводка

На этом завершается Общие сведения для обработки конфликтов параллелизма. Сведения о других способах обрабатывать различные сценарии параллелизма см. в разделе [оптимистичного параллелизма шаблоны](https://msdn.microsoft.com/en-us/data/jj592904) и [работа со значениями свойств](https://msdn.microsoft.com/en-us/data/jj592677) на сайте MSDN. Далее учебнике показано, как реализовать таблица на иерархию наследования для `Instructor` и `Student` сущности.

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
