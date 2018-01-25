---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: "Реализация оптимистической блокировки в SqlDataSource (VB) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы просмотрите essentials управления оптимистичным параллелизмом и исследуйте его с помощью элемента управления SqlDataSource применение."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 974ea50a0d12aae09107470815214b20068ea553
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Реализация оптимистической блокировки в SqlDataSource (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) или [скачать PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> В этом учебнике мы просмотрите essentials управления оптимистичным параллелизмом и исследуйте его с помощью элемента управления SqlDataSource применение.


## <a name="introduction"></a>Вступление

В предыдущем учебнике мы рассмотрели, как добавить вставки, обновления и удаления возможности для управления SqlDataSource. Иными словами, для обеспечения этих функций необходимо указать соответствующий `INSERT`, `UPDATE`, или `DELETE` инструкции SQL в элементе управления s `InsertCommand`, `UpdateCommand`, или `DeleteCommand` свойства, вместе с необходимым параметры в `InsertParameters`, `UpdateParameters`, и `DeleteParameters` коллекции. Хотя эти свойства и коллекции можно указать вручную, «мастер настройки источника данных s Дополнительно» предлагает сформировать `INSERT`, `UPDATE`, и `DELETE` на основе флажок инструкций, который будет автоматически создавать эти инструкции `SELECT` инструкции.

А также формирования `INSERT`, `UPDATE`, и `DELETE` флажок инструкций, в диалоговом окне Дополнительные параметры создания SQL включает параметр использования оптимистичного параллелизма (см. рис. 1). Если флажок установлен, `WHERE` предложения в автоматически сформированного `UPDATE` и `DELETE` инструкции изменены для только обновления или удаления если базовой t изменялся данных базы данных были изменены с момента пользователя последней загрузки данных в сетку.


![Вы можете добавить поддержку оптимистичный параллелизм с расширенными диалоговое окно параметров создания SQL](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Рис. 1**: вы можете добавить поддержку оптимистичный параллелизм с расширенными диалоговое окно параметров создания SQL


В [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) учебника мы рассмотрели основные принципы управления оптимистичным параллелизмом и как добавить его в элемент управления ObjectDataSource. В этом учебнике мы ретуширование основные моменты управления оптимистичным параллелизмом и исследуйте его с помощью SqlDataSource реализации.

## <a name="a-recap-of-optimistic-concurrency"></a>Повторение оптимистичного параллелизма

Для веб-приложений, позволяющих нескольким одновременно работающих пользователей, чтобы изменить или удалить те же данные, существует вероятность того, что один пользователь может случайно перезаписан другой s изменения. В [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) учебника были предоставлены в следующем примере:

Предположим, что два пользователя, Цзисунь и Сэм, одновременно открыли страницу приложения, позволяющего посетителям обновлять и удалять продукты с помощью элемента управления GridView. В то время как нажмите кнопку редактирования для товара. Цзисунь изменяет название продукта на Чай и нажатии кнопки "Обновить". Конечным результатом становится `UPDATE` отправляемая базу данных, которая задает *все* полей обновляемый продукт s (несмотря на то, что Цзисунь изменил только одно поле `ProductName`). На данный момент времени базы данных содержит значения Чай, категория напитков жидкости экзотические поставщика, и так далее для этого продукта. Тем не менее, на экране s Sam GridView по-прежнему показывает название продукта в редактируемой строке GridView как Chai. Через несколько секунд после Цзисунь s изменения были зафиксированы, Сэм обновляет категорию для приправы и последнее обновление. В результате `UPDATE` инструкции, отправляемые в базу данных, задает имя продукта Chai `CategoryID` соответствующий идентификатор категории приправы и т. д. Изменения s Цзисунь названия продукта были перезаписаны.

На рисунке 2 показано это взаимодействие.


[![Если два пользователя одновременно обновить запись существует s потенциальных для одного пользователя s изменений перезаписать другие устройства](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**На рисунке 2**: при двух пользователей одновременно обновление существует запись s потенциал для одного пользователя s изменений перезаписать другие устройства ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Чтобы предотвратить unfolding форму этот сценарий [управления параллелизмом](http://en.wikipedia.org/wiki/Concurrency_control) должен быть реализован. [Оптимистический параллелизм](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) фокус этого учебника работает на предположении, что конфликты параллелизма может быть и, большая часть времени, такой конфликт выиграл t возникнуть. Таким образом Если конфликт возникает, управление оптимистичным параллелизмом просто пользователю сохранить их изменения могут t, поскольку другой пользователь изменил те же данные.

> [!NOTE]
> Для приложений, где предполагается, что будет много конфликтов параллелизма, или если такие конфликты не являются приемлемой то управление пессимистичным параллелизмом может использоваться вместо этого. Обращаться к [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) учебника более подробное описание на управление пессимистичным параллелизмом.


Управление оптимистичным параллелизмом работает за счет того, что запись обновляемой или удаляемой имеет те же значения, как и при запуске процесса обновления или удаления. Например при нажатии кнопки «изменить» в изменяемого элемента управления GridView, запись s чтение из базы данных и отображения значений в текстовые поля и другие веб-элементов управления. Эти исходные значения сохраняются в GridView. Позже, после пользователь делает своих изменений и нажимает кнопку обновления `UPDATE` используется инструкция необходимо учитывать исходные и новые значения и обновить только базовой записи базы данных, если исходные значения, что пользователь начал редактирование совпадают со значениями в базе данных. На рисунке 3 показана эта последовательность событий.


[![Для инструкции Update или Delete для успешного выполнения и установка исходных значений должно быть равно текущими значениями базы данных](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Рис. 3**: для обновления или удаления выполняются успешно, исходные значения должен быть равен текущими значениями базы данных ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Существуют различные подходы к реализации оптимистичного параллелизма (см. [в статье](http://peterbromberg.net/) s [Optmistic параллелизма обновление логики](http://www.eggheadcafe.com/articles/20050719.asp) для кратко рассмотрим несколько параметров). Метод, используемый в SqlDataSource (а также ADO.NET типизированные наборы данных, используемые в наших уровня доступа к данным) расширяет `WHERE` предложение сравнением всех исходных значений. Следующие `UPDATE` инструкции, например, обновляет имя и цену продукта, только в том случае, если текущие значения базы данных равны значениям, которые изначально были получены при обновлении записи в GridView. `@ProductName` И `@UnitPrice` параметры содержат новые значения, введенные пользователем, в то время как `@original_ProductName` и `@original_UnitPrice` содержат значения, которые изначально были загружены в GridView, если была нажата кнопка "Изменить":


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Как будет показано в этом учебнике, Включение управления оптимистичным параллелизмом с SqlDataSource — простая установка флажка.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Шаг 1: Создание SqlDataSource, который поддерживает оптимистичный параллелизм

Сначала откройте `OptimisticConcurrency.aspx` страницу `SqlDataSource` папки. Перетащите элемент управления SqlDataSource из области элементов в конструктор, параметры его `ID` свойства `ProductsDataSourceWithOptimisticConcurrency`. Затем щелкните по ссылке настройки источника данных из элемента управления s смарт-тег. На первом экране мастера, выберите для работы с `NORTHWINDConnectionString` и нажмите кнопку Далее.


[![Выберите для работы с NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Рис. 4**: выберите для работы с `NORTHWINDConnectionString` ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


В этом примере мы будем добавлять GridView, который позволяет пользователям редактировать `Products` таблицы. Таким образом, Настройка экрана инструкции Select, выберите `Products` таблицу из раскрывающегося списка и выберите `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцов, как показано на рисунке 5.


[![Из таблицы Products возвращаемое ProductID, ProductName, UnitPrice и неподдерживаемые столбцы](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Рис. 5**: от `Products` таблицы, возвращаемые `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцы ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


После выбора столбцов, нажмите кнопку "Дополнительно", чтобы открыть диалоговое окно «Дополнительные параметры создания SQL». Проверьте формирования `INSERT`, `UPDATE`, и `DELETE` инструкций используйте флажки оптимистичного параллелизма и нажмите кнопку ОК (приведены обратно на рис. 1 снимок экрана). Завершите работу мастера, нажав кнопку Далее и Готово.

После завершения работы мастера настройки источника данных теперь пора проверить итоговое `DeleteCommand` и `UpdateCommand` свойства и `DeleteParameters` и `UpdateParameters` коллекции. Для этого проще всего щелкните на вкладке "источник" в левом нижнем углу для просмотра страницы s декларативного синтаксиса. Приведены `UpdateCommand` значение:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

С семь параметров в `UpdateParameters` коллекции:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Аналогичным образом `DeleteCommand` свойство и `DeleteParameters` коллекции должен выглядеть следующим образом:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Помимо расширения `WHERE` предложения `UpdateCommand` и `DeleteCommand` свойства (и добавление дополнительных параметров в коллекции соответствующего параметра), выбрав использование оптимистичного параллелизма параметр регулирует двух других Свойства:

- Изменения [ `ConflictDetection` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) из `OverwriteChanges` (по умолчанию) для`CompareAllValues`
- Изменения [ `OldValuesParameterFormatString` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) из {0} (по умолчанию) исходный\_{0}.

Когда данные веб-элемент управления вызывает SqlDataSource s `Update()` или `Delete()` метод, он передает исходные значения. Если SqlDataSource s `ConflictDetection` свойству `CompareAllValues`, эти исходные значения добавляются в команду. `OldValuesParameterFormatString` Свойство предоставляет шаблон именования, использовать эти исходные значения параметров. Мастер настройки источника данных использует исходное\_{0} и имена каждого исходного параметра `UpdateCommand` и `DeleteCommand` свойства и `UpdateParameters` и `DeleteParameters` коллекций соответствующим образом.

> [!NOTE]
> Поскольку мы повторно не использует s управления SqlDataSource, вставка возможности, вы можете удалить `InsertCommand` свойство и его `InsertParameters` коллекции.


## <a name="correctly-handlingnullvalues"></a>Для правильной обработки`NULL`значения

К сожалению, расширенная `UPDATE` и `DELETE` сделать автоматически инструкций, созданных с помощью мастера настройки источника данных при использовании оптимистичного параллелизма *не* работают с записи, содержащие `NULL` значения. Чтобы узнать, почему это происходит, попробуйте нашей s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice` Столбца в `Products` таблица может иметь `NULL` значения. Если имеет определенную запись `NULL` значение для `UnitPrice`, `WHERE` часть предложения `[UnitPrice] = @original_UnitPrice` будет *всегда* возвращают значение False, так как `NULL = NULL` всегда возвращает значение False. Таким образом, записи, содержащие `NULL` значения нельзя изменить или удалить, как `UPDATE` и `DELETE` инструкций `WHERE` предложений выиграл t возвращают все строки для обновления или удаления.

> [!NOTE]
> В июне 2004 г. в ошибке сначала отправлены в корпорацию Майкрософт [SqlDataSource приводит к возникновению ошибки неверные инструкций SQL](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) и сообщается планируется исправить в следующей версии ASP.NET.


Чтобы устранить эту проблему, необходимо вручную обновить `WHERE` предложений в обоих `UpdateCommand` и `DeleteCommand` свойства **все** столбцы, которые могут иметь `NULL` значения. Как правило, изменять `[ColumnName] = @original_ColumnName` для:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Это изменение можно непосредственно через декларативная разметка через параметры UpdateQuery или DeleteQuery из окна свойств или обновление и удаление вкладок в списке укажите пользовательские инструкции SQL или параметр хранимых процедур в данных, Настройка Мастер источников. Опять же, это изменение должно быть выполнено для *каждого* столбца в `UpdateCommand` и `DeleteCommand` s `WHERE` предложение, которое может содержать `NULL` значения.

Применение это в нашем примере результаты в следующий измененный `UpdateCommand` и `DeleteCommand` значения:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Шаг 2: Добавление элемента управления GridView, изменить и удалить параметры

SqlDataSource настроен для поддержки оптимистичного параллелизма все, что остается только добавить страницу, которая использует такое управление параллелизмом данных веб-элемента управления. В этом учебнике позволяют добавить GridView, предоставляет оба изменения и удаления функциональные возможности. Для этого перетащите элемент управления GridView с панели элементов в область конструктора и задайте его `ID` для `Products`. В смарт-теге s GridView, привяжите его к `ProductsDataSourceWithOptimisticConcurrency` элемент управления SqlDataSource, добавленный на шаге 1. Наконец проверьте параметры Разрешить изменение и включить удаление из смарт-тега.


[![Привязка GridView SqlDataSource и включить изменения и удаления](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Рис. 6**: привязка GridView SqlDataSource и разрешить изменение и удаление ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


После добавления в GridView, настроить его внешний вид, удалив `ProductID` BoundField, изменение `ProductName` BoundField s `HeaderText` свойство с продуктом и обновление `UnitPrice` BoundField, чтобы его `HeaderText` свойство просто цена. В идеальном случае мы d улучшения интерфейса для включения RequiredFieldValidator для редактирования `ProductName` значение и CompareValidator для `UnitPrice` значение (чтобы убедиться, что он s числовое значение в правильном формате). Ссылаться на [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебника для более подробное рассмотрение Настройка s GridView, интерфейс для редактирования.

> [!NOTE]
> GridView, в который должна быть включена s состояния представления, так как исходные значения передаются из GridView в SqlDataSource хранится в состоянии представления.


После внесения этих изменений в GridView, GridView и SqlDataSource должна выглядеть следующим образом:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Чтобы увидеть оптимистическое управление параллелизмом в действии, откройте два окна браузера и загрузить `OptimisticConcurrency.aspx` страницы как в среде. Нажмите кнопку Правка кнопки для первого продукта в обоих браузерах. В одном браузере названия продукта и нажмите кнопку обновления. Браузер выполнит обратную передачу, а GridView вернется в режим его до редактирования, отображение имени продукта для записи пользуйтесь кнопками.

Во втором окне браузера изменение цены (но оставить имя продукта, исходное значение) и нажмите кнопку обновления. При обратной передаче возвращает сетки для режима до редактирования, но изменения цены не записывается. Второй обозреватель показано совпадает со значением первое имя продукта с старая цена. Изменения, внесенные во второе окно браузера были потеряны. Кроме того изменения были потеряны довольно просто, как была не исключение или сообщение, указывающее, что нарушение параллелизма просто произошла.


[![Изменения во второе окно браузера были автоматически удалены](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Рис. 7**: изменения в второй браузера окна были автоматически потеряны ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


Причина, почему второй браузера s изменения не были зафиксированы был, так как `UPDATE` инструкцию s `WHERE` предложения отфильтровываются все записи и таким образом, была не затронула ни одной строки. Разрешить s рассмотрим `UPDATE` инструкцию:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Если второе окно браузера обновляет запись, исходное имя продукта указан в `WHERE` предложение t сопоставление с существующим именем product (поскольку он был изменен первый браузером). Таким образом, инструкция `[ProductName] = @original_ProductName` возвращает False и `UPDATE` не влияет на какие-либо записи.

> [!NOTE]
> Удалите работает таким же образом. В двух окнах браузера откройте запускаются, изменив данного продукта с одной и затем сохранение ее изменения. После сохранения изменений в одном браузере, нажмите кнопку «Удалить» для того же продукта в других. Поскольку исходные значения пункт не совпадать в `DELETE` инструкцию s `WHERE` предложение, удаление автоматически завершается ошибкой.


С точки зрения s конечного пользователя во второе окно браузера после нажатия кнопки "Обновить" сетки возвращает режиму до редактирования, но их изменения были потеряны. Тем не менее существует s не визуальную обратную связь, придерживайтесь t не существовал их изменения. В идеальном случае, если пользователь s изменения теряются, нарушение параллелизма, мы d уведомлением и, возможно, сохранить сетке в режиме редактирования. Позволяет увидеть, как это можно сделать s.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Шаг 3: Определение, когда произошло нарушение параллелизма

Так как нарушение параллелизма отклоняет изменения, внесенные одним, было бы неплохо для оповещения пользователя, когда произошло нарушение параллелизма. Для оповещения пользователя, s позволяют добавить метки веб-элемент управления в верхней части страницы, с именем `ConcurrencyViolationMessage` которого `Text` свойство отображает следующее сообщение: предпринята попытка обновить или удалить запись, которая одновременно был обновлен другим пользователем. Проверьте просмотрите изменений другого пользователя и затем повторить обновление или удаление. Задать элемент управления Label s `CssClass` свойства на "Предупреждение", который является классом CSS, определенного в `Styles.css` красного, курсив, полужирный и большого шрифта, который отображает текст. Наконец, задайте для свойства Label s `Visible` и `EnableViewState` свойства `False`. При этом будут скрыты метки, за исключением только обратных передач, где мы явно задать ее `Visible` свойства `True`.


[![Добавьте элемент управления Label на страницу, чтобы отобразить предупреждение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Рис. 8**: добавьте элемент управления Label на страницу, чтобы отобразить предупреждение ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


При выполнении обновления или удаления, GridView s `RowUpdated` и `RowDeleted` обработчики событий срабатывать после его источника данных было выполнено запрошенного update или delete. Можно определить количество строк, затронутых операцией из этих обработчиков событий. Если ни одна строка не изменены, мы хотим отображать `ConcurrencyViolationMessage` метки.

Создайте обработчик событий для обоих `RowUpdated` и `RowDeleted` события и добавьте следующий код:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

В обоих обработчиков событий проверьте `e.AffectedRows` свойства и, если он равен 0, значение `ConcurrencyViolationMessage` метки s `Visible` свойства `True`. В `RowUpdated` обработчика событий, мы также, чтобы оставаться в режиме редактирования, задав GridView `KeepInEditMode` значение true. При этом необходимо повторно привязать данные к сетке, чтобы другие данные пользователя s загружается в интерфейс редактирования. Это достигается путем вызова GridView s `DataBind()` метод.

Как показано на рис. 9, с помощью этих двух обработчиков очень заметных сообщение отображается каждый раз, когда происходит нарушение параллелизма.


[![Сообщение отображается в случае одновременного нарушения](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Рис. 9**: сообщение при возникновении нарушение параллелизма ([Просмотр полноразмерное изображение](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Сводка

При создании веб-приложение, где нескольких параллельных пользователей, изменить те же данные, важно рассмотреть варианты управления параллелизмом. По умолчанию данные ASP.NET, веб-элементов управления и элементов управления источниками данных они не отражают любой элемент управления параллелизмом. Как было показано в этом учебнике, реализация управления оптимистичным параллелизмом с SqlDataSource относительно легко и быстро. SqlDataSource обрабатывает большую часть подготовительная работа для вашего Добавление дополнить `WHERE` предложений, чтобы автоматически сформированного `UPDATE` и `DELETE` операторов, но возникла, несколько тонкостей при обработке `NULL` значения столбцов, как описано в Для правильной обработки `NULL` значения раздела.

Этот учебник завершает изучение SqlDataSource. К работе с данными, с помощью многоуровневой архитектуры и ObjectDataSource возвращает оставшиеся учебные видеоматериалы.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Назад](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
