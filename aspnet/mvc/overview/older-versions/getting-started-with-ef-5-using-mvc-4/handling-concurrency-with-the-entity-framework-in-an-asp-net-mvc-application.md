---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Обработка параллелизма с помощью Entity Framework в приложении ASP.NET MVC (7 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 07d4d673b7bb6bad6e9d8cbacbc965a60608db2a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829885"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Обработка параллелизма с помощью Entity Framework в приложении ASP.NET MVC (7 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В двух предыдущих руководствах вы работали со связанными данными. Этом руководстве показано, как обеспечивать параллелизм. Вы создадите веб-страниц, которые работают с `Department` сущности и страницы, изменять и удалять `Department` сущностей будет обрабатывать ошибки параллелизма. На следующих рисунках показаны страницы индекса и Delete, включая некоторые сообщения, которые отображаются при возникновении конфликта параллелизма.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь отображает данные сущности, чтобы изменить их, а другой пользователь обновляет данные той же сущности до того, как изменение первого пользователя будет записано в базу данных. Если не включить обнаружение таких конфликтов, то пользователь, обновляющий базу данных последним, перезаписывает изменения другого пользователя. Во многих приложениях такой риск допустим: при небольшом числе пользователей или обновлений, а также в случае, если перезапись некоторых изменений не является критической, стоимость реализации параллелизма может перевесить его преимущества. В этом случае вам не нужно настраивать приложение для обработки конфликтов параллелизма.

### <a name="pessimistic-concurrency-locking"></a>Пессимистичный параллелизм (блокировка)

Если приложению нужно предотвратить случайную потерю данных в сценариях параллелизма, одним из способов сделать это являются блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед чтением строки из базы данных вы запрашиваете блокировку для доступа для обновления или только для чтения. Если заблокировать строку для обновления, другие пользователи не могут заблокировать ее для обновления или только для чтения, так как получат копию данных, которые находятся в процессе изменения. Если заблокировать строку только для чтения, другие пользователи также могут заблокировать ее только для чтения, но не для обновления.

Управление блокировками имеет недостатки. Оно может оказаться сложным с точки зрения программирования. Требуются значительные ресурсы базы данных управления, и он может привести к снижению производительности как количество пользователей приложения увеличивается (то есть он плохо масштабируется). Поэтому не все системы управления базами данных поддерживают пессимистичный параллелизм. Платформа Entity Framework предоставляет нет встроенной поддержки и здесь не показано, как реализовать его.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Альтернативой пессимистичному параллелизму является *оптимистичного параллелизма*. Оптимистическая блокировка допускает появление конфликтов параллелизма, а затем обрабатывает их соответствующим образом. Например, Джон запускает отделов изменения страницы, изменения **бюджета** сумма для кафедры английского языка с 350 000,00 USD на 0,00 USD.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Прежде чем Джон выбирает **Сохранить**, Мария запускает один и тот же странице и изменения **Дата начала** поле из 9/1/2007 до 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Джон выбирает **Сохранить** первый и видит, его изменение, когда браузер возвращается на страницу индекса, а затем Мария нажимает кнопку **Сохранить**. Дальнейший ход событий определяется порядком обработки конфликтов параллелизма. Некоторые параметры перечислены ниже:

- Вы можете отслеживать, для какого свойства пользователь изменил и обновил только соответствующие столбцы в базе данных. В этом примере сценария данные не будут потеряны, так как эти два пользователя обновляли разные свойства. Далее время кто-то кафедру английского языка, он увидит изменения Джон и Джейн — Дата начала 8/8/2013 и бюджет в нуль долларов США.

    Этот метод обновления помогает снизить число конфликтов, которые могут привести к потере данных, но не позволяет избежать такой потери, когда конкурирующие изменения вносятся в одно свойство сущности. То, работает ли Entity Framework в таком режиме, зависит от того, как вы реализуете код обновления. В веб-приложении это часто нецелесообразно, так как может потребоваться обрабатывать большой объем состояний, чтобы отслеживать все исходные значения свойств для сущности, а также новые значения. Обслуживание большого объема состояний может повлиять на производительность приложения, так как она требует ресурсов сервера или должен быть включен в веб-страницы сам (например, в скрытые поля).
- Вы можете разрешить изменение Джейн перезаписать изменения. Далее время кто-то кафедру английского языка, он увидит 8/8/2013 и восстановленное значение 350 000,00 USD. Такой подход называется *победой клиента* или *сохранением последнего внесенного изменения*. (Значения клиента имеют приоритет над в хранилище данных). Как отмечено во введении к этому разделу, если вы не пишете код для обработки параллелизма, она выполняется автоматически.
- Можно запретить изменение Джейн Дмитрия в базе данных. Как правило будет отображать сообщение об ошибке, Показать ее текущее состояние данных и предоставить ей повторно применить изменения, если она по-прежнему хочет сделать их. Это называется *победой хранилища*. (Значения в хранилище имеют приоритет над данными, передаваемыми клиентом.) В этом руководстве вы реализуете сценарий победы хранилища. Данный метод гарантирует, что никакие изменения не перезаписываются без оповещения пользователя о случившемся.

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

Конфликты можно разрешать путем обработки [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) создаваемые исключения платформы Entity Framework. Чтобы определить, когда именно нужно выдавать исключения, платформа Entity Framework должна быть в состоянии обнаруживать конфликты. Поэтому нужно соответствующим образом настроить базу данных и модель данных. Ниже приведены некоторые варианты для реализации обнаружения конфликтов:

- Включите в таблицу базы данных столбец отслеживания, который позволяет определять, когда была изменена строка. Затем можно настроить Entity Framework для включения этого столбца в `Where` предложение SQL `Update` или `Delete` команды.

    Тип данных столбца отслеживания обычно является [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) значение — это число, увеличивающееся каждый раз при обновлении строки. В `Update` или `Delete` команде `Where` предложение содержит исходное значение столбца отслеживания (исходную версию строки). Если обновляемая строка была изменена другим пользователем, значение в `rowversion` столбца отличается от исходного значения, поэтому `Update` или `Delete` инструкции не удается найти строку для обновления из-за `Where` предложение. Когда платформа Entity Framework находит, что строки не были обновлены с `Update` или `Delete` команды (то есть когда число затронутых строк равно нулю), она интерпретирует это как конфликт параллелизма.
- Настройка Entity Framework для включения исходных значений каждого столбца в таблице в `Where` предложении `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилось с момента первого чтения `Where` предложение не возвращает строку для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Для таблиц базы данных со множеством столбцов этот подход может привести к очень больших `Where` предложений и может потребоваться обрабатывать большой объем состояний. Как отмечалось ранее, обслуживание большого объема состояний могут ухудшить производительность приложения, так как она требует ресурсов сервера или должны быть включены в сам веб-страницы. Поэтому этот подход обычно не рекомендуется и не метод, используемый в этом руководстве.

    Если вы хотите реализовать этот подход к параллелизму, необходимо пометить все свойства первичного ключа в сущность, которую необходимо отслеживать параллелизм, добавив [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) к ним атрибут. То, что изменение позволяет Entity Framework для включения всех столбцов в инструкции SQL, `WHERE` предложении `UPDATE` инструкций.

В оставшейся части этого руководства вам предстоит добавить [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) свойство для отслеживания `Department` сущности, создание контроллера и представлений и проверить, что все работает правильно.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Добавить свойство оптимистичного параллелизма в сущность Department

В *Models\Department.cs*, добавьте свойство отслеживания `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) атрибут указывает, что этот столбец будет включен в `Where` предложении `Update` и `Delete` команды, отправляемые в базу данных. Этот атрибут называется [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) так, как предыдущие версии SQL Server использовали SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) тип данных SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) заменил его. Тип .net для `rowversion` является массив байтов. Если вы предпочитаете использовать текучий API, можно использовать [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) метод, чтобы задать свойства отслеживания, как показано в следующем примере:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Добавив свойство, вы изменили модель базы данных, поэтому нужно выполнить еще одну миграцию. Введите в консоли диспетчера пакетов (PMC) следующие команды:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Создание контроллера Department

Создание `Department` контроллера и представлений так же, как другие контроллеры, используя следующие параметры:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

В *Controllers\DepartmentController.cs*, добавьте `using` инструкции:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Изменение «LastName» на «FullName» повсюду в этом файле (четыре вхождения) так, чтобы списки раскрывающегося списка администратор отдела будет содержать полное имя преподавателя, а не только фамилию.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Замените существующий код для `HttpPost` `Edit` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Представление будет хранить исходный `RowVersion` значение в скрытое поле. Когда связыватель модели создает `department` экземпляра, этот объект будет иметь исходный `RowVersion` значение свойства и новые значения для других свойств, введенных пользователем на странице "Edit". Затем в том случае, когда платформа Entity Framework создает SQL `UPDATE` команды, что команда будет включать `WHERE` предложение, которое ищет строку, содержащую исходный `RowVersion` значение.

Если строки не затрагивает `UPDATE` команды (нет строк, имеющих исходное `RowVersion` значение), Entity Framework выдает `DbUpdateConcurrencyException` исключение и код в `catch` блок возвращает затронутую сущность `Department` сущности из исключения объект. Эта сущность содержит значения, считанные из базы данных и новые значения, введенные пользователем:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Затем код добавляет пользовательское сообщение об ошибке для каждого столбца, для которого значения базы данных отличается от пользователь ввел на странице "Edit":

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Длинное сообщение об ошибке объясняется, что произошло и что делать о нем:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Наконец, код задает `RowVersion` значение `Department` извлечь объект новое значение из базы данных. Это новое значение `RowVersion` будет сохранено в скрытом поле при повторном отображении страницы "Edit" (Редактирование). Когда пользователь в следующий раз нажимает кнопку **Save** (Сохранить), перехватываются только те ошибки параллелизма, которые возникли с момента повторного отображения страницы "Edit" (Редактирование).

В *Views\Department\Edit.cshtml*, добавьте скрытое поле для сохранения `RowVersion` значение свойства, сразу после скрытого поля для `DepartmentID` свойства:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

В *Views\Department\Index.cshtml*, замените существующий код следующим кодом для перемещения ссылок строки слева и изменения страницы названием и заголовками столбцов для отображения `FullName` вместо `LastName` в **Администратора** столбца:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Тестирование обработки оптимистичного параллелизма

Запустите сайт и нажмите кнопку **отделов**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Щелкните правой кнопкой мыши **изменить** гиперссылки для Kim Abercrombie и выберите **открыть на новой вкладке** щелкните **изменить** гиперссылку для Kim Abercrombie. В двух окнах отображаются те же сведения.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Измените поле в первом окне браузера и нажмите кнопку **Сохранить**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

В браузере отображается страница индекса с измененным значением.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Измените любое поле в окне второго обозревателя и нажмите кнопку **Сохранить**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Нажмите кнопку **Сохранить** во втором окне браузера. Отображается сообщение об ошибке:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Снова нажмите кнопку **Save** (Сохранить). Значение, введенное на второй обозреватель сохраняется вместе с исходное значение данных, которые вы изменяете на первый браузер. Сохраненные значения отображаются при открытии страницы индекса.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Обновление страницы "Delete"

Для страницы "Delete" (Удаление) платформа Entity Framework обнаруживает конфликты параллелизма, вызванные схожим изменением кафедры. Когда `HttpGet` `Delete` метод отображает представление подтверждения, оно содержит исходное `RowVersion` значение в скрытое поле. Затем это значение становится доступными для `HttpPost` `Delete` метод, вызываемый при подтверждении удаления пользователем. Когда платформа Entity Framework создает SQL `DELETE` команды, он включает `WHERE` предложение с исходным `RowVersion` значение. Если команда ни одной строки не затрагивает (то есть строка была изменена после отображения страницы подтверждения удаления), возникает исключение параллелизма и `HttpGet Delete` метод вызывается с флаг ошибки `true` чтобы повторно отобразить страница подтверждения с сообщением об ошибке. Это также возможно, что отсутствие затронутых строк, так как строка была удалена другим пользователем, поэтому в этом случае отображается другое сообщение об ошибке.

В *DepartmentController.cs*, замените `HttpGet` `Delete` метод следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Этот метод принимает необязательный параметр, который указывает, отображается ли страница повторно после ошибки параллелизма. Если этот флаг `true`, сообщение об ошибке отправляется в представление с помощью `ViewBag` свойство.

Замените код в `HttpPost` `Delete` метод (с именем `DeleteConfirmed`) следующим кодом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

В шаблонном коде, который вы только что заменили, этот метод принимал только идентификатор записи:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Вы изменили этот параметр, чтобы `Department` экземпляр сущности, созданную связывателем модели. Это дает доступ к `RowVersion` значение свойства в дополнение к ключу записи.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Вы также изменили имя метода действия с `DeleteConfirmed` на `Delete`. Шаблонный код с именем `HttpPost` `Delete` метод `DeleteConfirmed` для предоставления `HttpPost` уникальную сигнатуру метода. (Среда CLR требует, чтобы перегруженные методы имели разные параметры метода.) Теперь, когда сигнатуры являются уникальными, можно придерживаться соглашения MVC и использовать то же имя для `HttpPost` и `HttpGet` удалить методы.

При перехвате ошибки параллелизма код повторно отображает страницу подтверждения удаления и предоставляет флаг, указывающий, что нужно отобразить сообщение об ошибке параллелизма.

В *Views\Department\Delete.cshtml*, замените шаблонный код следующим кодом, который делает Форматирование изменяет и добавляет поле сообщения об ошибке. Изменения выделены.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Этот код добавляет сообщение об ошибке между `h2` и `h3` заголовки:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Он заменяет `LastName` с `FullName` в `Administrator` поля:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Наконец, он добавляет скрытые поля для `DepartmentID` и `RowVersion` свойства после `Html.BeginForm` инструкции:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Откройте страницу индекса кафедр. Щелкните правой кнопкой мыши **удалить** гиперссылки для кафедры английского языка и выберите **открыть в новом окне** щелкните в первом окне **изменить** гиперссылки для английского языка отдел.

В первом окне измените одно из значений и нажмите кнопку **Сохранить** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Страница индекса подтверждает изменения.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Во втором окне нажмите кнопку **удалить**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Вы видите сообщение об ошибке параллелизма, а значения кафедры обновляются с использованием актуальных сведений из базы данных.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Если нажать кнопку **Delete** (Удалить) еще раз, вы будете перенаправлены на страницу индекса, которая показывает, что кафедра была удалена.

## <a name="summary"></a>Сводка

На этом заканчивается введение в обработку конфликтов параллелизма. Сведения о других способах обрабатывать различные сценарии параллелизма, см. в разделе [оптимистичного параллелизма шаблоны](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) и [работа со значениями свойств](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) в блоге группы Entity Framework. Следующее руководство посвящено реализации таблица на иерархию наследования для `Instructor` и `Student` сущностей.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
