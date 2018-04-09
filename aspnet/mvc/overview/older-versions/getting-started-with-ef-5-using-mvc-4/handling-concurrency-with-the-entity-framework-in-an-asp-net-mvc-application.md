---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Обработка параллелизма с платформой Entity Framework в приложении ASP.NET MVC (7, 10) | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 609f493845f1d00a47d175a1b623a7f4866d191e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Обработка параллелизма с платформой Entity Framework в приложении ASP.NET MVC (7, 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В двух предыдущих занятий вы работали с соответствующими данными. Этот учебник демонстрирует обработки параллелизма. Вы создадите веб-страниц, которые работают с `Department` сущности и страниц, изменение и удаление `Department` сущностей будет обрабатывать ошибки параллелизма. На следующих рисунках страницы индекса и Delete, включая некоторые сообщения, которые отображаются, если возникает конфликт параллелизма.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь отображает данные сущности, чтобы изменить их, а другой пользователь обновляет данные той же сущности до того, как изменение первого пользователя будет записано в базу данных. Если не включить обнаружение таких конфликтов, то пользователь, обновляющий базу данных последним, перезаписывает изменения другого пользователя. Во многих приложениях такой риск допустим: при небольшом числе пользователей или обновлений, а также в случае, если перезапись некоторых изменений не является критической, стоимость реализации параллелизма может перевесить его преимущества. В этом случае вам не нужно настраивать приложение для обработки конфликтов параллелизма.

### <a name="pessimistic-concurrency-locking"></a>Пессимистическая блокировка (блокировки)

Если приложению нужно предотвратить случайную потерю данных в сценариях параллелизма, одним из способов сделать это являются блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед чтением строки из базы данных вы запрашиваете блокировку для доступа для обновления или только для чтения. Если заблокировать строку для обновления, другие пользователи не могут заблокировать ее для обновления или только для чтения, так как получат копию данных, которые находятся в процессе изменения. Если заблокировать строку только для чтения, другие пользователи также могут заблокировать ее только для чтения, но не для обновления.

Управление блокировками имеет недостатки. Оно может оказаться сложным с точки зрения программирования. Она требует значительные ресурсы базы данных управления, и он может вызвать снижение производительности как число пользователей приложения увеличивает (то есть, он плохо масштабируется). Поэтому не все системы управления базами данных поддерживают пессимистичный параллелизм. Платформа Entity Framework предоставляет нет встроенная поддержка и учебнике не показано, как для его реализации.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Вместо него следует использовать для пессимистичный параллелизм *оптимистичного параллелизма*. Оптимистическая блокировка допускает появление конфликтов параллелизма, а затем обрабатывает их соответствующим образом. Например, Джон запускает отделов изменить страницу, изменения **бюджета** сумма для английского языка отдела из 350,000.00 $ $ 0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Прежде чем Джон выбирает **Сохранить**, Мария запускает одну и ту же страницу и изменения **Дата начала** поля из 9/1/2007 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Джон выбирает **Сохранить** первым и его изменение браузер по возвращении на страницу индекса, а затем Мария щелкает видит **Сохранить**. Дальнейший ход событий определяется порядком обработки конфликтов параллелизма. Некоторые параметры перечислены ниже:

- Вы можете отслеживать, для какого свойства пользователь изменил и обновил только соответствующие столбцы в базе данных. В этом примере сценария данные не будут потеряны, так как эти два пользователя обновляли разные свойства. Далее время кто-то просматривает отделе английского языка, они видят изменения Джон и Джейн — Начальная дата 8/8/2013 и бюджета ноль долларов.

    Этот метод обновления помогает снизить число конфликтов, которые могут привести к потере данных, но не позволяет избежать такой потери, когда конкурирующие изменения вносятся в одно свойство сущности. То, работает ли Entity Framework в таком режиме, зависит от того, как вы реализуете код обновления. В веб-приложении это часто нецелесообразно, так как может потребоваться обрабатывать большой объем состояний, чтобы отслеживать все исходные значения свойств для сущности, а также новые значения. Обслуживание больших объемов состояния может повлиять на производительность приложения, так как он требует ресурсы сервера или должны быть включены на странице веб-(например, в скрытых полях).
- Можно перезаписать изменения Джона изменение Джейн. Далее время кто-то просматривает отделе английского языка, они видят 8/8/2013 и восстановленное значение $350,000.00. Такой подход называется *победой клиента* или *сохранением последнего внесенного изменения*. (Значения клиента имеют приоритет над возможности хранилища данных). Как отмечено во введении к этому разделу, если вы не пишете код для обработки параллелизма, она выполняется автоматически.
- Можно запретить изменение Джейн обновляется в базе данных. Обычно сообщение об ошибке, Показать его текущее состояние данных и разрешить его для повторного применения своих изменений, если она по-прежнему хочет сделать их. Это называется *победой хранилища*. (Значения в хранилище имеют приоритет над данными, передаваемыми клиентом.) В этом руководстве вы реализуете сценарий победы хранилища. Данный метод гарантирует, что никакие изменения не перезаписываются без оповещения пользователя о случившемся.

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

Конфликты можно разрешать путем обработки [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) исключения, которые выдает Entity Framework. Чтобы определить, когда именно нужно выдавать исключения, платформа Entity Framework должна быть в состоянии обнаруживать конфликты. Поэтому нужно соответствующим образом настроить базу данных и модель данных. Ниже приведены некоторые варианты для реализации обнаружения конфликтов:

- Включите в таблицу базы данных столбец отслеживания, который позволяет определять, когда была изменена строка. Затем можно настроить для включения этого столбца в Entity Framework `Where` предложение SQL `Update` или `Delete` команд.

    Тип данных столбца отслеживания обычно является [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) значение — это число, увеличивающееся каждый раз при обновлении строки. В `Update` или `Delete` команды `Where` предложение содержит исходное значение столбца отслеживания (версии). Если обновляемой строке был изменен другим пользователем, значение в `rowversion` столбца отличается от исходного значения, поэтому `Update` или `Delete` инструкции не удается найти строку для обновления из-за `Where` предложения. Когда Entity Framework находит, что строки не были обновлены с `Update` или `Delete` команды (то есть, когда количество задействованных строк равно нулю), он интерпретирует как конфликт параллелизма.
- Настройка включения исходных значений для каждого из столбцов в таблице в платформе Entity Framework `Where` предложения `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилась с момента прочтите строки `Where` предложение не возвращает строки для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Для таблиц базы данных, имеющие много столбцов, этот подход может привести к очень больших `Where` предложений и может потребоваться поддерживать большие объемы состояния. Как отмечалось ранее, обслуживание больших объемов состояния могут ухудшить производительность приложения, так как он требует ресурсы сервера или должен быть включен в саму веб-страницу. Поэтому этот подход обычно не рекомендуется, а не метод, используемый в этом учебнике.

    Если необходимо реализовать этот подход к параллелизма, необходимо пометить все свойства первичного ключа в сущность, которую необходимо отслеживать параллелизм для добавляя [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) к ним атрибут. То, что изменение позволяет платформе Entity Framework включают все столбцы в инструкции SQL, `WHERE` предложения `UPDATE` инструкции.

В оставшейся части этого учебника вам предстоит добавить [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) свойство для отслеживания `Department` сущности, создать контроллер и представления и проверить, что все работает правильно.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Добавить свойство оптимистичного параллелизма для сущности «отдел»

В *Models\Department.cs*, добавьте отслеживания свойство с именем `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) атрибут указывает, что этот столбец будет включен в `Where` предложения `Update` и `Delete` команды, отправляемые в базу данных. Атрибут называется [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) из-за предыдущих версий SQL Server используется SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) типа данных перед SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) заменил его. Тип .net для `rowversion` — массив байтов. Если вы предпочитаете использовать fluent API, можно использовать [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) метод, чтобы задать свойства трассировки, как показано в следующем примере:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Добавив свойство, вы изменили модель базы данных, поэтому нужно выполнить еще одну миграцию. Введите в консоли диспетчера пакетов (PMC) следующие команды:

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

Наконец, код задает `RowVersion` значение `Department` извлечь объект новое значение из базы данных. Это новое значение `RowVersion` будет сохранено в скрытом поле при повторном отображении страницы "Edit" (Редактирование). Когда пользователь в следующий раз нажимает кнопку **Save** (Сохранить), перехватываются только те ошибки параллелизма, которые возникли с момента повторного отображения страницы "Edit" (Редактирование).

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

В браузере отображается страница индекса с измененным значением.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Измените любое поле в второе окно браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Нажмите кнопку **Сохранить** во второе окно браузера. Отображается сообщение об ошибке:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Снова нажмите кнопку **Save** (Сохранить). Значение, введенное на второй обозреватель сохраняется вместе с исходное значение данных, изменения в браузере первой. Сохраненные значения отображаются при открытии страницы индекса.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы удаления

Для страницы "Delete" (Удаление) платформа Entity Framework обнаруживает конфликты параллелизма, вызванные схожим изменением кафедры. Когда `HttpGet` `Delete` метод отображает представление подтверждения, представление включает в себя исходный `RowVersion` значения в скрытом поле. Значение затем становится доступным `HttpPost` `Delete` метод, который вызывается, когда пользователь подтверждения удаления. Если Entity Framework создает SQL `DELETE` команды, он включает `WHERE` предложение с первоначальным `RowVersion` значение. Если результаты команды в ноль строк влияет (то есть строка была изменена после отображается страница подтверждения удаления), исключение параллелизма и `HttpGet Delete` метод вызывается с ошибка установлен флаг `true` для повторного отображения страница подтверждения с сообщением об ошибке. Также возможно, что нулевые затронуты, так как строка была удалена другим пользователем, поэтому в этом случае отображается другое сообщение об ошибке.

В *DepartmentController.cs*, замените `HttpGet` `Delete` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Этот метод принимает необязательный параметр, который указывает, отображается ли страница повторно после ошибки параллелизма. Если этот флаг `true`, сообщение об ошибке отправляется в представлении с помощью `ViewBag` свойство.

Замените код в `HttpPost` `Delete` метод (с именем `DeleteConfirmed`) следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

В шаблонном коде, который вы только что заменили, этот метод принимал только идентификатор записи:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Вы изменили этот параметр, чтобы `Department` экземпляр сущности, созданные связывателя модели. Это дает доступ к `RowVersion` значение свойства помимо ключа записи.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Формирования шаблонов код, называемый `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` метод уникальная сигнатура. (Среда CLR требует, чтобы перегруженные методы имели разные параметры метода.) Теперь, когда сигнатурах являются уникальными, можно не покидайте соглашение MVC и использовать то же имя для `HttpPost` и `HttpGet` удаления методов.

При перехвате ошибки параллелизма код повторно отображает страницу подтверждения удаления и предоставляет флаг, указывающий, что нужно отобразить сообщение об ошибке параллелизма.

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

Вы видите сообщение об ошибке параллелизма, а значения кафедры обновляются с использованием актуальных сведений из базы данных.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Если нажать кнопку **Delete** (Удалить) еще раз, вы будете перенаправлены на страницу индекса, которая показывает, что кафедра была удалена.

## <a name="summary"></a>Сводка

На этом заканчивается введение в обработку конфликтов параллелизма. Сведения о других способах обрабатывать различные сценарии параллелизма см. в разделе [оптимистичного параллелизма шаблоны](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) и [работа со значениями свойств](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) блоге группы разработчиков платформы Entity Framework. Далее учебнике показано, как реализовать таблица на иерархию наследования для `Instructor` и `Student` сущности.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
