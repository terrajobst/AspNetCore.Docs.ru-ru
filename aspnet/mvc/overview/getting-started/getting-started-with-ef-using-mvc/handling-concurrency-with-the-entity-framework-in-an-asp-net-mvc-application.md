---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Обработка параллелизма с помощью Entity Framework 6 в приложении ASP.NET MVC 5 (10 из 12) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: riande
ms.date: 12/08/2014
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 708c38b8e0815c1d8b899c4d5a6f878e235340bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824114"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Обработка параллелизма с помощью Entity Framework 6 в приложении ASP.NET MVC 5 (10 из 12)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущих учебниках вы узнали, как обновлять данные. Это руководство описывает, как обрабатывать конфликты, когда несколько пользователей одновременно изменяют одну сущность.

Вы измените веб-страниц, которые работают с `Department` сущности, чтобы они обрабатывают ошибки параллелизма. На следующих рисунках показаны страницы индекса и Delete, включая некоторые сообщения, которые отображаются при возникновении конфликта параллелизма.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь отображает данные сущности, чтобы изменить их, а другой пользователь обновляет данные той же сущности до того, как изменение первого пользователя будет записано в базу данных. Если не включить обнаружение таких конфликтов, то пользователь, обновляющий базу данных последним, перезаписывает изменения другого пользователя. Во многих приложениях такой риск допустим: при небольшом числе пользователей или обновлений, а также в случае, если перезапись некоторых изменений не является критической, стоимость реализации параллелизма может перевесить его преимущества. В этом случае вам не нужно настраивать приложение для обработки конфликтов параллелизма.

### <a name="pessimistic-concurrency-locking"></a>Пессимистичный параллелизм (блокировка)

Если приложению нужно предотвратить случайную потерю данных в сценариях параллелизма, одним из способов сделать это являются блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед чтением строки из базы данных вы запрашиваете блокировку для доступа для обновления или только для чтения. Если заблокировать строку для обновления, другие пользователи не могут заблокировать ее для обновления или только для чтения, так как получат копию данных, которые находятся в процессе изменения. Если заблокировать строку только для чтения, другие пользователи также могут заблокировать ее только для чтения, но не для обновления.

Управление блокировками имеет недостатки. Оно может оказаться сложным с точки зрения программирования. Оно требует значительных ресурсов управления базами данных, а также может вызвать проблемы с производительностью по мере увеличения числа пользователей приложения. Поэтому не все системы управления базами данных поддерживают пессимистичный параллелизм. Платформа Entity Framework предоставляет нет встроенной поддержки и здесь не показано, как реализовать его.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Альтернативой пессимистичному параллелизму является *оптимистичного параллелизма*. Оптимистическая блокировка допускает появление конфликтов параллелизма, а затем обрабатывает их соответствующим образом. Например, Джон запускает отделов изменения страницы, изменения **бюджета** сумма для кафедры английского языка с 350 000,00 USD на 0,00 USD.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Прежде чем Джон выбирает **Сохранить**, Мария запускает один и тот же странице и изменения **Дата начала** поле из 9/1/2007 до 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Джон выбирает **Сохранить** первый и видит, его изменение, когда браузер возвращается на страницу индекса, а затем Мария нажимает кнопку **Сохранить**. Дальнейший ход событий определяется порядком обработки конфликтов параллелизма. Некоторые параметры перечислены ниже:

- Вы можете отслеживать, для какого свойства пользователь изменил и обновил только соответствующие столбцы в базе данных. В этом примере сценария данные не будут потеряны, так как эти два пользователя обновляли разные свойства. Далее время кто-то кафедру английского языка, он увидит изменения Джон и Джейн — Дата начала 8/8/2013 и бюджет в нуль долларов США.

    Этот метод обновления помогает снизить число конфликтов, которые могут привести к потере данных, но не позволяет избежать такой потери, когда конкурирующие изменения вносятся в одно свойство сущности. То, работает ли Entity Framework в таком режиме, зависит от того, как вы реализуете код обновления. В веб-приложении это часто нецелесообразно, так как может потребоваться обрабатывать большой объем состояний, чтобы отслеживать все исходные значения свойств для сущности, а также новые значения. Обработка большого объема состояний может повлиять на производительность приложения, так как требует ресурсов сервера или должна быть включена непосредственно в веб-страницу (например, в скрытые поля) или файл cookie.
- Вы можете разрешить изменение Джейн перезаписать изменения. Далее время кто-то кафедру английского языка, он увидит 8/8/2013 и восстановленное значение 350 000,00 USD. Такой подход называется *победой клиента* или *сохранением последнего внесенного изменения*. (Все значения из клиента имеют приоритет над данными в хранилище.) Как отмечено во введении к этому разделу, если вы не пишете код для обработки параллелизма, она выполняется автоматически.
- Можно запретить изменение Джейн Дмитрия в базе данных. Как правило будет отображать сообщение об ошибке, Показать ее текущее состояние данных и предоставить ей повторно применить изменения, если она по-прежнему хочет сделать их. Это называется *победой хранилища*. (Значения в хранилище имеют приоритет над данными, передаваемыми клиентом.) В этом руководстве вы реализуете сценарий победы хранилища. Данный метод гарантирует, что никакие изменения не перезаписываются без оповещения пользователя о случившемся.

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

Конфликты можно разрешать путем обработки [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) создаваемые исключения платформы Entity Framework. Чтобы определить, когда именно нужно выдавать исключения, платформа Entity Framework должна быть в состоянии обнаруживать конфликты. Поэтому нужно соответствующим образом настроить базу данных и модель данных. Ниже приведены некоторые варианты для реализации обнаружения конфликтов:

- Включите в таблицу базы данных столбец отслеживания, который позволяет определять, когда была изменена строка. Затем можно настроить Entity Framework для включения этого столбца в `Where` предложение SQL `Update` или `Delete` команды.

    Тип данных столбца отслеживания обычно является [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) значение — это число, увеличивающееся каждый раз при обновлении строки. В `Update` или `Delete` команде `Where` предложение содержит исходное значение столбца отслеживания (исходную версию строки). Если обновляемая строка была изменена другим пользователем, значение в `rowversion` столбца отличается от исходного значения, поэтому `Update` или `Delete` инструкции не удается найти строку для обновления из-за `Where` предложение. Когда платформа Entity Framework находит, что строки не были обновлены с `Update` или `Delete` команды (то есть когда число затронутых строк равно нулю), она интерпретирует это как конфликт параллелизма.
- Настройка Entity Framework для включения исходных значений каждого столбца в таблице в `Where` предложении `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилось с момента первого чтения `Where` предложение не возвращает строку для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Для таблиц базы данных со множеством столбцов этот подход может привести к очень больших `Where` предложений и может потребоваться обрабатывать большой объем состояний. Как было указано ранее, обслуживание большого объема состояний может негативно повлиять на производительность приложения. Поэтому в общем случае данный подход не рекомендуется, кроме того, он не применяется и в этом руководстве.

    Если вы хотите реализовать этот подход к параллелизму, необходимо пометить все свойства первичного ключа в сущность, которую необходимо отслеживать параллелизм, добавив [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) к ним атрибут. То, что изменение позволяет Entity Framework для включения всех столбцов в инструкции SQL, `WHERE` предложении `UPDATE` инструкций.

В оставшейся части этого руководства вам предстоит добавить [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) свойство для отслеживания `Department` сущности, создание контроллера и представлений и проверить, что все работает правильно.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Добавить свойство оптимистичного параллелизма в сущность Department

В *Models\Department.cs*, добавьте свойство отслеживания `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) атрибут указывает, что этот столбец будет включен в `Where` предложении `Update` и `Delete` команды, отправляемые в базу данных. Этот атрибут называется [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) так, как предыдущие версии SQL Server использовали SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) тип данных SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) заменил его. Тип .net для [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) является массив байтов.

Если вы предпочитаете использовать текучий API, можно использовать [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) метод, чтобы задать свойства отслеживания, как показано в следующем примере:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Добавив свойство, вы изменили модель базы данных, поэтому нужно выполнить еще одну миграцию. Введите в консоли диспетчера пакетов (PMC) следующие команды:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Изменение контроллера Department

В *Controllers\DepartmentController.cs*, добавьте `using` инструкции:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

В *DepartmentController.cs* измените все четыре вхождения «LastName» на «FullName», чтобы раскрывающиеся списки администратор отдела будет содержать полное имя преподавателя, а не только фамилию.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Замените существующий код для `HttpPost` `Edit` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Если метод `FindAsync` возвращает значение null, кафедра была удалена другим пользователем. Код, показанный использует значения из отправленной формы для создания сущности кафедры, таким образом, чтобы можно было отобразить повторно страницы "Edit" с сообщением об ошибке. Кроме того, повторно создать сущность кафедры не нужно, если вы выводите только сообщение об ошибке без повторного отображения полей кафедры.

Представление сохраняет исходное `RowVersion` его в получает значение в скрытое поле, а метод `rowVersion` параметра. Перед вызовом `SaveChanges` нужно поместить это исходное значение свойства `RowVersion` в коллекцию `OriginalValues` для сущности. Затем в том случае, когда платформа Entity Framework создает SQL `UPDATE` команды, что команда будет включать `WHERE` предложение, которое ищет строку, содержащую исходный `RowVersion` значение.

Если строки не затрагивает `UPDATE` команды (нет строк, имеющих исходное `RowVersion` значение), Entity Framework выдает `DbUpdateConcurrencyException` исключение и код в `catch` блок возвращает затронутую сущность `Department` сущности из исключения объект.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Этот объект содержит новые значения, введенные пользователем в его `Entity` , а также значения можно получить на чтение из базы данных, вызвав `GetDatabaseValues` метод.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues` Метод возвращает значение null, если кто-то удалил строку из базы данных; в противном случае вам придется привести возвращенный объект к `Department` класса, чтобы получить доступ к `Department` свойства. (Так как вы уже проверено для удаления, `databaseEntry` будет иметь значение null, только в том случае, если Кафедра была удалена после `FindAsync` выполняет и перед `SaveChanges` выполняет.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Затем код добавляет пользовательское сообщение об ошибке для каждого столбца, для которого значения базы данных отличается от пользователь ввел на странице "Edit":

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Длинное сообщение об ошибке объясняется, что произошло и что делать о нем:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Наконец, код задает `RowVersion` значение `Department` извлечь объект новое значение из базы данных. Это новое значение `RowVersion` будет сохранено в скрытом поле при повторном отображении страницы "Edit" (Редактирование). Когда пользователь в следующий раз нажимает кнопку **Save** (Сохранить), перехватываются только те ошибки параллелизма, которые возникли с момента повторного отображения страницы "Edit" (Редактирование).

В *Views\Department\Edit.cshtml*, добавьте скрытое поле для сохранения `RowVersion` значение свойства, сразу после скрытого поля для `DepartmentID` свойства:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Тестирование обработки оптимистичного параллелизма

Запустите сайт и нажмите кнопку **отделов**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Щелкните правой кнопкой мыши **изменить** гиперссылки для кафедры английского языка и выберите **открыть на новой вкладке** щелкните **изменить** гиперссылку для кафедры английского языка. На двух вкладках отображаются те же сведения.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Измените поле на первой вкладке браузера и нажмите кнопку **Save** (Сохранить).

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

В браузере отображается страница индекса с измененным значением.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Измените поле на второй вкладке браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Нажмите кнопку **Сохранить** на второй вкладке браузера. Отображается сообщение об ошибке:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Снова нажмите кнопку **Save** (Сохранить). Значение, введенное на второй вкладке браузера сохраняется вместе с исходное значение данных, которые были изменены в первом браузера. Сохраненные значения отображаются при открытии страницы индекса.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы "Delete"

Для страницы "Delete" (Удаление) платформа Entity Framework обнаруживает конфликты параллелизма, вызванные схожим изменением кафедры. Когда `HttpGet` `Delete` метод отображает представление подтверждения, оно содержит исходное `RowVersion` значение в скрытое поле. Затем это значение становится доступными для `HttpPost` `Delete` метод, вызываемый при подтверждении удаления пользователем. Когда платформа Entity Framework создает SQL `DELETE` команды, он включает `WHERE` предложение с исходным `RowVersion` значение. Если команда ни одной строки не затрагивает (то есть строка была изменена после отображения страницы подтверждения удаления), возникает исключение параллелизма и `HttpGet Delete` метод вызывается с флаг ошибки `true` чтобы повторно отобразить страница подтверждения с сообщением об ошибке. Это также возможно, что отсутствие затронутых строк, так как строка была удалена другим пользователем, поэтому в этом случае отображается другое сообщение об ошибке.

В *DepartmentController.cs*, замените `HttpGet` `Delete` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Этот метод принимает необязательный параметр, который указывает, отображается ли страница повторно после ошибки параллелизма. Если этот флаг `true`, сообщение об ошибке отправляется в представление с помощью `ViewBag` свойство.

Замените код в `HttpPost` `Delete` метод (с именем `DeleteConfirmed`) следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

В шаблонном коде, который вы только что заменили, этот метод принимал только идентификатор записи:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Вы изменили этот параметр, чтобы `Department` экземпляр сущности, созданную связывателем модели. Это дает доступ к `RowVersion` значение свойства в дополнение к ключу записи.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Шаблонный код с именем `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` уникальную сигнатуру метода. (Среда CLR требуется перегруженные методы имели разные параметры метода). Теперь, когда сигнатуры являются уникальными, можно придерживаться соглашения MVC и использовать то же имя для `HttpPost` и `HttpGet` удалить методы.

При перехвате ошибки параллелизма код повторно отображает страницу подтверждения удаления и предоставляет флаг, указывающий, что нужно отобразить сообщение об ошибке параллелизма.

В *Views\Department\Delete.cshtml*, замените шаблонный код следующим кодом, который добавляет поле сообщения об ошибке и скрытые поля для свойств DepartmentID и RowVersion. Изменения выделены.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Этот код добавляет сообщение об ошибке между `h2` и `h3` заголовки:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Он заменяет `LastName` с `FullName` в `Administrator` поля:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Наконец, он добавляет скрытые поля для `DepartmentID` и `RowVersion` свойства после `Html.BeginForm` инструкции:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Откройте страницу индекса кафедр. Щелкните правой кнопкой мыши **удалить** гиперссылки для кафедры английского языка и выберите **открыть на новой вкладке** на первой вкладке щелкните **изменить** гиперссылку для кафедры английского языка.

В первом окне измените одно из значений и нажмите кнопку **Сохранить** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Страница индекса подтверждает изменения.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

На второй вкладке нажмите кнопку **Delete** (Удалить).

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Вы видите сообщение об ошибке параллелизма, а значения кафедры обновляются с использованием актуальных сведений из базы данных.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Если нажать кнопку **Delete** (Удалить) еще раз, вы будете перенаправлены на страницу индекса, которая показывает, что кафедра была удалена.

## <a name="summary"></a>Сводка

На этом заканчивается введение в обработку конфликтов параллелизма. Сведения о других способах обрабатывать различные сценарии параллелизма, см. в разделе [оптимистичного параллелизма шаблоны](https://msdn.microsoft.com/data/jj592904) и [работа со значениями свойств](https://msdn.microsoft.com/data/jj592677) на сайте MSDN. Следующее руководство посвящено реализации таблица на иерархию наследования для `Instructor` и `Student` сущностей.

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
