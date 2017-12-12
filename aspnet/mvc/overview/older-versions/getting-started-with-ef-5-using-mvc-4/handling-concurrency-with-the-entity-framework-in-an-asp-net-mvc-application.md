---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Обработка параллелизма с платформой Entity Framework в приложении ASP.NET MVC (7, 10) | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b072134043ceda809bfeca98447a132ed407b323
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Обработка параллелизма с платформой Entity Framework в приложении ASP.NET MVC (7, 10)
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В двух предыдущих занятий вы работали с соответствующими данными. Этот учебник демонстрирует обработки параллелизма. Вы создадите веб-страниц, которые работают с `Department` сущности и страниц, изменение и удаление `Department` сущностей будет обрабатывать ошибки параллелизма. На следующих рисунках страницы индекса и Delete, включая некоторые сообщения, которые отображаются, если возникает конфликт параллелизма.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь отображены данные сущности для его изменения, а затем другой пользователь обновляет данные той же сущности перед изменение первый пользователь будет записано в базу данных. Если не включить обнаружение такие конфликты, кто обновляет базу данных, перезаписывает изменения другого пользователя. Во многих приложениях приемлемо этот риск: при наличии нескольких пользователей или несколько обновлений, или если не действительно являются критическими, если некоторые изменения будут перезаписаны, стоимость программирования для параллелизма может перевесить преимущества. В этом случае не нужно настроить приложение для обработки конфликтов параллелизма.

### <a name="pessimistic-concurrency-locking"></a>Пессимистическая блокировка (блокировки)

Если приложению требуется предотвратить случайную потерю данных в сценарии параллелизма, один из способов сделать это — использовать блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед прочтением строки из базы данных запрашивается блокировка для только для чтения или для доступ для обновления. При блокировке строку для обновления доступ другие пользователи не разрешены для блокировки строки, либо для только для чтения или обновления доступа, так как он может получить копию данных, которая находится в процессе изменения. При блокировке строк для доступа только для чтения, другие также можно блокировать его для доступа только для чтения, но не для обновления.

Управление блокировками имеет недостатки. Он может оказаться непростой задачей программы. Она требует значительные ресурсы базы данных управления, и он может вызвать снижение производительности как число пользователей приложения увеличивает (то есть, он плохо масштабируется). По этим причинам не всех систем управления базами данных поддерживают пессимистичного параллелизма. Платформа Entity Framework предоставляет нет встроенная поддержка и учебнике не показано, как для его реализации.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Вместо него следует использовать для пессимистичный параллелизм *оптимистичного параллелизма*. Оптимистический параллелизм означает разрешение конфликтов параллелизма активна и затем отклик соответствующим образом, если они есть. Например, Джон запускает отделов изменить страницу, изменения **бюджета** сумма для английского языка отдела из 350,000.00 $ $ 0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Прежде чем Джон выбирает **Сохранить**, Мария запускает одну и ту же страницу и изменения **Дата начала** поля из 9/1/2007 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Джон выбирает **Сохранить** первым и его изменение браузер по возвращении на страницу индекса, а затем Мария щелкает видит **Сохранить**. Дальнейший ход событий определяется порядок обработки конфликтов параллелизма. Ниже приведены некоторые из параметров:

- Можно хранить список какие свойства были изменены пользователем и обновлять соответствующие столбцы в базе данных. В примере сценария данные не будут потеряны, поскольку различные свойства были обновлены двумя пользователями. Далее время кто-то просматривает отделе английского языка, они видят изменения Джон и Джейн — Начальная дата 8/8/2013 и бюджета ноль долларов.

    Этот метод обновления может снизить количество конфликтов, которые могут привести к потере данных, но его не удается избежать потери данных, если конкурирующих изменений в то же свойство сущности. Работает ли платформа Entity Framework следующим образом зависит от того, как реализовать код обновления. Нецелесообразно часто веб-приложения, так как он может потребоваться поддерживать большое количество состояний, чтобы отслеживать все исходные значения свойств для сущности, а также новые значения. Обслуживание больших объемов состояния может повлиять на производительность приложения, так как он требует ресурсы сервера или должны быть включены на странице веб-(например, в скрытых полях).
- Можно перезаписать изменения Джона изменение Джейн. Далее время кто-то просматривает отделе английского языка, они видят 8/8/2013 и восстановленное значение $350,000.00. Это называется *клиент побеждает* или *побеждает последний* сценария. (Значения клиента имеют приоритет над возможности хранилища данных). Как отмечено во введении к этому разделу, если этого не сделать, написания кода для обработки параллелизма, она выполняется автоматически.
- Можно запретить изменение Джейн обновляется в базе данных. Обычно сообщение об ошибке, Показать его текущее состояние данных и разрешить его для повторного применения своих изменений, если она по-прежнему хочет сделать их. Это называется *побеждает хранилища* сценария. (Значения хранилища данных имеют приоритет над значениями, отправленные клиентом). В этом учебнике необходимо реализовать сценарий Wins хранилища. Этот метод гарантирует, что изменения не переписываются без пользователя предупреждения о происходящем.

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

Конфликты можно разрешать путем обработки [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) исключения, которые выдает Entity Framework. Чтобы определить, когда необходимо создавать эти исключения, Entity Framework необходима возможность обнаружения конфликтов. Таким образом необходимо настроить базу данных и модели данных соответствующим образом. Ниже приведены некоторые параметры для включения обнаружения конфликтов:

- В таблице базы данных включают столбец отслеживания, который может использоваться для определения момента изменения строки. Затем можно настроить для включения этого столбца в Entity Framework `Where` предложение SQL `Update` или `Delete` команд.

    Тип данных столбца отслеживания обычно является [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) значение — это число, увеличивающееся каждый раз при обновлении строки. В `Update` или `Delete` команды `Where` предложение содержит исходное значение столбца отслеживания (версии). Если обновляемой строке был изменен другим пользователем, значение в `rowversion` столбца отличается от исходного значения, поэтому `Update` или `Delete` инструкции не удается найти строку для обновления из-за `Where` предложения. Когда Entity Framework находит, что строки не были обновлены с `Update` или `Delete` команды (то есть, когда количество задействованных строк равно нулю), он интерпретирует как конфликт параллелизма.
- Настройка включения исходных значений для каждого из столбцов в таблице в платформе Entity Framework `Where` предложения `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилась с момента прочтите строки `Where` предложение не возвращает строки для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Для таблиц базы данных, имеющие много столбцов, этот подход может привести к очень больших `Where` предложений и может потребоваться поддерживать большие объемы состояния. Как отмечалось ранее, обслуживание больших объемов состояния могут ухудшить производительность приложения, так как он требует ресурсы сервера или должен быть включен в саму веб-страницу. Поэтому этот подход обычно не рекомендуется, а не метод, используемый в этом учебнике.

    Если необходимо реализовать этот подход к параллелизма, необходимо пометить все свойства первичного ключа в сущность, которую необходимо отслеживать параллелизм для добавляя [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) к ним атрибут. То, что изменение позволяет платформе Entity Framework включают все столбцы в инструкции SQL, `WHERE` предложения `UPDATE` инструкции.

В оставшейся части этого учебника вам предстоит добавить [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) свойство для отслеживания `Department` сущности, создать контроллер и представления и проверить, что все работает правильно.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Добавить свойство оптимистичного параллелизма для сущности «отдел»

В *Models\Department.cs*, добавьте отслеживания свойство с именем `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) атрибут указывает, что этот столбец будет включен в `Where` предложения `Update` и `Delete` команды, отправляемые в базу данных. Атрибут называется [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) из-за предыдущих версий SQL Server используется SQL [timestamp](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) типа данных перед SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) заменил его. Тип .net для `rowversion` — массив байтов. Если вы предпочитаете использовать fluent API, можно использовать [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) метод, чтобы задать свойства трассировки, как показано в следующем примере:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

При добавлении свойства изменить модель базы данных, необходимо выполнить еще один процесс миграции. В пакет Диспетчер консоли (PMC), введите следующие команды:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Создание контроллера отдела

Создание `Department` контроллера и представления так же, как другие контроллеры со следующими параметрами:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

В *Controllers\DepartmentController.cs*, добавьте `using` инструкции:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Изменение «LastName» на «FullName» везде в этом файле (четыре вхождений) так, чтобы списки раскрывающегося списка администратор отдела будет содержать полное имя инструктора, а не просто фамилию.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Замените существующий код для `HttpPost` `Edit` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Представление будет хранить исходные `RowVersion` значения в скрытом поле. Если создается связывателя модели `department` экземпляра, этот объект будет иметь исходный `RowVersion` значение свойства, а новые значения для других свойств, как введенные пользователем на странице «Изменение». Затем в том случае, когда платформа Entity Framework создает SQL `UPDATE` команды, команда будет включать `WHERE` предложение, которое ищет строку, которая содержит исходный `RowVersion` значение.

Если строки не затронуты `UPDATE` команда (строки не имеют исходной `RowVersion` значение), Entity Framework создает исключение `DbUpdateConcurrencyException` исключения, а код в `catch` блок получает соответствующие `Department` сущности из исключения объект. Эта сущность содержит значения, считанные из базы данных и новые значения, введенные пользователем:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Затем код добавляет сообщения о пользовательской ошибке для каждого столбца, имеющего значений базы данных, отличающийся от введенного пользователем на странице «Изменение»:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Длинное сообщение об ошибке объясняется, что произошло и что делать о нем.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Наконец, код задает `RowVersion` значение `Department` извлечь объект новое значение из базы данных. Этот новый `RowVersion` значение будет храниться в скрытое поле, когда изменение, снова откроется страница и следующий раз пользователь щелкает **Сохранить**, только ошибки параллелизма, которые происходят с момента будет порождено повторно отобразить страницы правки.

В *Views\Department\Edit.cshtml*, добавьте скрытое поле, чтобы сохранить `RowVersion` значение свойства, следующий сразу за скрытого поля для `DepartmentID` свойства:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

В *Views\Department\Index.cshtml*, замените существующий код следующим кодом для перемещения ссылки строк слева и изменения страницы заголовок и заголовки столбцов для отображения `FullName` вместо `LastName` в **Администратора** столбца:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Тестирование обработки оптимистичного параллелизма

Запустите сайт и нажмите кнопку **отделы**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Щелкните правой кнопкой мыши **изменить** гиперссылку для Kim Abercrombie и выберите **открыть в новой вкладке** щелкните **изменить** гиперссылку для Kim Abercrombie. В двух окнах отображаются те же сведения.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Измените поля в первом окне браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

В браузере будет отображена страница индекса с измененное значение.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Измените любое поле в второе окно браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Нажмите кнопку **Сохранить** во второе окно браузера. Вы видите сообщение об ошибке:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Нажмите кнопку **Сохранить** еще раз. Значение, введенное на второй обозреватель сохраняется вместе с исходное значение данных, изменения в браузере первой. Появиться сохраненные значения при появлении страницы индекса.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы удаления

На странице Delete Entity Framework обнаруживает конфликты параллелизма, причиной else редактирования отделе так же, как. Когда `HttpGet` `Delete` метод отображает представление подтверждения, представление включает в себя исходный `RowVersion` значения в скрытом поле. Значение затем становится доступным `HttpPost` `Delete` метод, который вызывается, когда пользователь подтверждения удаления. Если Entity Framework создает SQL `DELETE` команды, он включает `WHERE` предложение с первоначальным `RowVersion` значение. Если результаты команды в ноль строк влияет (то есть строка была изменена после отображается страница подтверждения удаления), исключение параллелизма и `HttpGet Delete` метод вызывается с ошибка установлен флаг `true` для повторного отображения страница подтверждения с сообщением об ошибке. Также возможно, что нулевые затронуты, так как строка была удалена другим пользователем, поэтому в этом случае отображается другое сообщение об ошибке.

В *DepartmentController.cs*, замените `HttpGet` `Delete` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Этот метод принимает необязательный параметр, который указывает, выполняется ли страница отобразится после ошибки параллелизма. Если этот флаг `true`, сообщение об ошибке отправляется в представлении с помощью `ViewBag` свойство.

Замените код в `HttpPost` `Delete` метод (с именем `DeleteConfirmed`) следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

В создании кода, который Вы заменили этот метод принято только идентификатор записи:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Вы изменили этот параметр, чтобы `Department` экземпляр сущности, созданные связывателя модели. Это дает доступ к `RowVersion` значение свойства помимо ключа записи.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Изменились имя метода действия из `DeleteConfirmed` для `Delete`. Формирования шаблонов код, называемый `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` метод уникальная сигнатура. (Среда CLR требуется перегруженные методы, чтобы иметь параметры другой метод). Теперь, когда сигнатурах являются уникальными, можно не покидайте соглашение MVC и использовать то же имя для `HttpPost` и `HttpGet` удаления методов.

При одновременном доступе, код заново отобразит страницу подтверждения удаления и предоставляет флаг, указывающий, он должен отображать сообщение об ошибке с параллелизмом.

В *Views\Department\Delete.cshtml*, replace формирования шаблонов код следующим кодом, который делает некоторые параметры форматирования, изменения и добавляет поля сообщения ошибки. Изменения выделены.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Этот код добавляет сообщение об ошибке между `h2` и `h3` заголовки:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Он заменяет `LastName` с `FullName` в `Administrator` поля:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Наконец, он добавляет скрытые поля для `DepartmentID` и `RowVersion` свойства после `Html.BeginForm` инструкции:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Запустите страницу индекса отделов. Щелкните правой кнопкой мыши **удаление** гиперссылку для английского языка подразделение и выберите **открыть в новом окне** в первом окне выберите **изменить** гиперссылки для английского языка отдел.

В первом окне, измените одно из значений и нажмите кнопку **Сохранить** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Страница индекса подтверждает изменения.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Во втором окне щелкните **удалить**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Вы видите сообщение об ошибке с параллелизмом, а отдел значения обновляются с возможности в базе данных.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Если щелкнуть **удалить** еще раз, будет перенаправлен на страницу индекса, которая показывает, отдел был удален.

## <a name="summary"></a>Сводка

На этом завершается Общие сведения для обработки конфликтов параллелизма. Сведения о других способах обрабатывать различные сценарии параллелизма см. в разделе [оптимистичного параллелизма шаблоны](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) и [работа со значениями свойств](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) блоге группы разработчиков платформы Entity Framework. Далее учебнике показано, как реализовать таблица на иерархию наследования для `Instructor` и `Student` сущности.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
