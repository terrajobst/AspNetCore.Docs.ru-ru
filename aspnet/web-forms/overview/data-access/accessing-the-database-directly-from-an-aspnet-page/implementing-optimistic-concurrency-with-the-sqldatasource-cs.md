---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Реализация оптимистичного параллелизма с помощью элемента управления SqlDataSource (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы просмотрите основные оптимистическое управление параллелизмом и исследуйте способы реализации с помощью элемента управления SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f2590e8e7712d719eb89403ef839f03066a93d2b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839180"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Реализация оптимистичного параллелизма с помощью элемента управления SqlDataSource (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) или [скачать PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> В этом руководстве мы просмотрите основные оптимистическое управление параллелизмом и исследуйте способы реализации с помощью элемента управления SqlDataSource.


## <a name="introduction"></a>Вступление

В предыдущем учебном курсе было рассмотрено, как добавить Вставка, обновление и удаление возможностей для элемента управления SqlDataSource. Иными словами, чтобы предоставить эти функции необходимо было указывать соответствующий `INSERT`, `UPDATE`, или `DELETE` инструкции SQL в элементе управления s `InsertCommand`, `UpdateCommand`, или `DeleteCommand` свойства, а также соответствующий параметры в `InsertParameters`, `UpdateParameters`, и `DeleteParameters` коллекций. Хотя эти свойства и коллекции можно указать вручную, «мастер настройки источника данных s Дополнительно» предлагает создать `INSERT`, `UPDATE`, и `DELETE` на основе инструкций флажок, который будет автоматически создавать эти инструкции `SELECT` инструкции.

А также формирования `INSERT`, `UPDATE`, и `DELETE` флажок инструкций, в диалоговом окне Дополнительные параметры создания SQL включает параметр использования оптимистичного параллелизма (см. рис. 1). Если флажок установлен, `WHERE` предложения в автоматически сформированного `UPDATE` и `DELETE` инструкции изменены для только обновления или удаления если базовой t посещавший данных базы данных был изменен после пользователя последней загрузки данных в сетку.


![Вы можете добавить поддержку оптимистичный параллелизм с расширенными диалоговое окно параметров Создание кода SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Рис. 1**: вы можете добавить поддержку оптимистичный параллелизм с расширенными диалоговое окно параметров Создание кода SQL


Вернитесь в [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) учебнике мы рассмотрели основные принципы управления оптимистичным параллелизмом и как добавить его к элементу ObjectDataSource. В этом руководстве мы ретуширование основные управление оптимистичным параллелизмом и исследуйте его с помощью элемента управления SqlDataSource реализации.

## <a name="a-recap-of-optimistic-concurrency"></a>Краткий обзор оптимистичного параллелизма

Для веб-приложений, позволяющих нескольким одновременно работающих пользователей, чтобы изменить или удалить те же данные, существует вероятность, что один пользователь может случайно перезаписать другой изменения s. В [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) учебник, предоставленный мною приведенный ниже:

Представьте себе, что два пользователя, Цзисунь и Сэм, одновременно открыли страницу приложения, позволяющего посетителям обновлять и удалять продукты с помощью элемента управления GridView. И нажмите кнопку "Изменить" для продукта Chai примерно то же время. Цзисунь изменяет название продукта на «Чай Chai» и нажимает кнопку кнопкой "Обновить". Конечным результатом является `UPDATE` инструкцию, которая отправляется в базу данных, которая задает *все* обновляемые поля продукта s (хотя Цзисунь изменил только одно поле `ProductName`). На данный момент база данных содержит значения «Чай Chai», категория «Напитки», поставщик Exotic Liquids и так далее для данного конкретного продукта. Тем не менее GridView на экране s Sam по-прежнему отображается название продукта в редактируемой строке GridView как Chai. Через несколько секунд после изменения Цзисунь s были зафиксированы, Сэм обновляет категорию на «Специи» и нажимает кнопку обновления. В результате `UPDATE` инструкции, отправляемые в базу данных, которая устанавливает название продукта Chai, `CategoryID` соответствующий идентификатор категории Специи и т. д. Цзисунь s изменения названия продукта были перезаписаны.

На рисунке 2 показано это взаимодействие.


[![При одновременном обновлении записи существует вероятность s s один пользователь изменяет перезаписать другие устройства](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Рис. 2**: при двух пользователей одновременно обновление существует запись s потенциал для одного пользователя s изменений перезаписать другие устройства ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Чтобы предотвратить возникновение такой ситуации из unfolding разновидность [управления параллелизмом](http://en.wikipedia.org/wiki/Concurrency_control) должен быть реализован. [Оптимистичный параллелизм](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) фокус данного учебника работает на предположении, что конфликты параллелизма может быть и, подавляющее большинство случаев такие конфликты выиграл t возникают. Таким образом Если конфликт возникает, управление оптимистичным параллелизмом просто пользователю сохранить их изменения удается так, как и те же данные были изменены другим пользователем.

> [!NOTE]
> Для приложений, где предполагается, что будет много конфликтов параллелизма, или если такие конфликты не являются приемлемой затем пессимистическое управление параллелизмом можно использовать вместо этого. Вернуться к [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) учебника более глубокое обсуждение управление пессимистичным параллелизмом.


Управление оптимистичным параллелизмом осуществляется путем обеспечения, что запись обновляемой или удаляемой имеет те же значения, как при запуске процесса обновления или удаления. Например при нажатии кнопки "Изменить" в изменяемого элемента управления GridView, значения запись s чтение из базы данных и отображаются в текстовые поля и другие веб-элементов управления. Эти исходные значения сохраняются элементом GridView. Позже, после того как пользователь вносит изменения ее и щелкает кнопкой "Обновить", `UPDATE` используется инструкция необходимо учитывать исходные и новые значения и только обновление базовой записи в базе данных, если исходные значения, что пользователь начал редактирование идентичны со значениями в базе данных. На рисунке 3 показана эта последовательность событий.


[![Для обновления или удаления, для успешного выполнения исходные значения должен быть равным текущие значения базы данных](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Рис. 3**: For Update или Delete, чтобы добиться успеха, исходные значения должен быть равным текущие значения базы данных ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Существуют различные подходы к реализации оптимистичного параллелизма (см. в разделе [статье](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic параллелизма обновление логики](http://www.eggheadcafe.com/articles/20050719.asp) для кратко рассмотрим несколько вариантов). Методика элемента управления SqlDataSource (а также ADO.NET типизированные наборы данных, используемые в наших уровня доступа к данным) расширяет `WHERE` предложение сравнением всех исходных значений. Следующие `UPDATE` инструкция, к примеру, обновляет название и цену продукта, только в том случае, если текущие значения базы данных равны значениям, которые были изначально получены при обновлении записи GridView. `@ProductName` И `@UnitPrice` параметры содержат новые значения, введенные пользователем, в то время как `@original_ProductName` и `@original_UnitPrice` содержат значения, которые были изначально загружены в GridView, если была нажата кнопка редактирования:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Как мы увидим в этом руководстве, включения управления оптимистичным параллелизмом с помощью элемента управления SqlDataSource сложнее, чем нужно установить флажок.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Шаг 1: Создание элемента управления SqlDataSource, поддерживающего оптимистичный параллелизм

Сначала откройте `OptimisticConcurrency.aspx` странице из `SqlDataSource` папки. Перетащите элемент управления SqlDataSource из инструментария в конструктор, параметры его `ID` свойства `ProductsDataSourceWithOptimisticConcurrency`. Затем щелкните ссылку Настройка источника данных в смарт-теге элемента управления s. На первом экране мастера, выберите для работы с `NORTHWINDConnectionString` и нажмите кнопку Далее.


[![Выберите для работы с NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Рис. 4**: выберите для работы с `NORTHWINDConnectionString` ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


В этом примере мы добавим GridView, который позволяет пользователям редактировать `Products` таблицы. Таким образом, Настройка экрана инструкции Select, выберите `Products` таблицу из раскрывающегося списка и выберите `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцов, как показано на рис. 5.


[![Из таблицы Products возвращаемое ProductID, ProductName, UnitPrice и неподдерживаемые столбцы](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Рис. 5**: от `Products` таблицы, возвращаемые `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцы ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


После выбора столбцов, нажмите кнопку «Дополнительно», чтобы открыть диалоговое окно Дополнительные параметры создания SQL. Проверьте формирования `INSERT`, `UPDATE`, и `DELETE` инструкций и используйте флажки оптимистичного параллелизма и нажмите кнопку OK (см рис. 1 показана). Завершите работу мастера, нажав кнопку Далее, а затем завершите.

После завершения работы мастера настройки источника данных Отвлекитесь и просмотрите полученный в результате `DeleteCommand` и `UpdateCommand` свойства и `DeleteParameters` и `UpdateParameters` коллекций. Самый простой способ сделать это — щелкните на вкладке "источник" в левом нижнем углу для декларативного синтаксиса страницы s см. в разделе. Там вы найдете `UpdateCommand` значение:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

С семью параметрами в `UpdateParameters` коллекции:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Аналогичным образом `DeleteCommand` свойство и `DeleteParameters` коллекции должен выглядеть следующим образом:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Кроме предоставления дополнительных `WHERE` предложения `UpdateCommand` и `DeleteCommand` свойства (и добавление дополнительных параметров в коллекции соответствующих параметров), выбрав использование оптимистичного параллелизма параметр регулирует двух других Свойства:

- Изменения [ `ConflictDetection` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) из `OverwriteChanges` (по умолчанию) для `CompareAllValues`
- Изменения [ `OldValuesParameterFormatString` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) из {0} (по умолчанию) исходным\_ {0} .

Когда данные веб-элемент управления вызывает SqlDataSource s `Update()` или `Delete()` метод, он передает в исходные значения. Если SqlDataSource s `ConflictDetection` свойству `CompareAllValues`, эти исходные значения добавляются к команде. `OldValuesParameterFormatString` Свойство предоставляет шаблон именования, используемый для эти исходные значения параметров. Мастер настройки источника данных использует исходное\_ {0} и имена каждого исходного параметра в `UpdateCommand` и `DeleteCommand` свойства и `UpdateParameters` и `DeleteParameters` коллекций соответствующим образом.

> [!NOTE]
> Так как мы повторно не с помощью элемента управления SqlDataSource CTRL + s, вставка возможности, вы можете удалить `InsertCommand` свойство и его `InsertParameters` коллекции.


## <a name="correctly-handlingnullvalues"></a>Для правильной обработки`NULL`значения

К сожалению, дополненной `UPDATE` и `DELETE` у автоматически созданных инструкций с помощью мастера настройки источника данных при использовании оптимистичного параллелизма *не* работают с записи, содержащие `NULL` значения. Чтобы понять, почему, рассмотрим наши s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

`UnitPrice` Столбца в `Products` таблица может иметь `NULL` значения. Если имеет определенную запись `NULL` значение `UnitPrice`, `WHERE` часть предложения `[UnitPrice] = @original_UnitPrice` будет *всегда* имеют значение False, так как `NULL = NULL` всегда возвращает значение False. Таким образом, записи, содержащие `NULL` значения невозможно изменить или удалить, как `UPDATE` и `DELETE` инструкций `WHERE` предложения выиграл t возвращают все строки, чтобы обновить или удалить.

> [!NOTE]
> Эта ошибка сначала было передано в корпорацию Майкрософт в июне 2004 в [SqlDataSource создает неправильный инструкций SQL](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) и сообщается, что планируется исправить в следующей версии ASP.NET.


Чтобы устранить эту проблему, необходимо вручную обновить `WHERE` предложений в обоих `UpdateCommand` и `DeleteCommand` свойства **все** столбцы, которые могут иметь `NULL` значения. Как правило, изменять `[ColumnName] = @original_ColumnName` для:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Это изменение можно сделать непосредственно через декларативную разметки, в параметрах UpdateQuery или DeleteQuery из окна свойств или через обновление и удаление вкладок в списке укажите пользовательские инструкции SQL или параметр хранимых процедур в данных настройки Мастер источников. Опять же, это изменение должно быть выполнено для *каждые* столбца в `UpdateCommand` и `DeleteCommand` s `WHERE` предложение, которое может содержать `NULL` значения.

Применяя его к нашему примеру приводит следующий измененный `UpdateCommand` и `DeleteCommand` значения:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Шаг 2: Добавление элемента GridView с помощью редактирования и удаления параметров

С помощью элемента управления SqlDataSource, настроен для поддержки оптимистичного параллелизма остается только добавить данных веб-элемента управления на страницу, который использует этот элемент управления параллелизмом. Для этого руководства позволяют s добавьте элемент управления GridView, который предоставляет режимах редактирования и функцию удаления. Для этого перетащите GridView с панели элементов в конструктор и задайте его `ID` для `Products`. Смарт-теге GridView s, привяжите его к `ProductsDataSourceWithOptimisticConcurrency` управления SqlDataSource, добавленный на шаге 1. Наконец проверьте параметры включить редактирование и разрешить удаление, в смарт-теге.


[![Привязка элемента управления SqlDataSource GridView и разрешить правку и удаление](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Рис. 6**: привязки GridView к SqlDataSource и разрешить редактирование и удаление ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


После добавления элемента GridView, настроить его внешний вид, удалив `ProductID` BoundField, изменение `ProductName` BoundField s `HeaderText` свойство для продукта и ее обновления `UnitPrice` BoundField таким образом, чтобы его `HeaderText` свойство просто цена. В идеальном случае мы d улучшения интерфейса редактирования для включения RequiredFieldValidator для `ProductName` значение и CompareValidator для `UnitPrice` значение (чтобы гарантировать, что s числовое значение в правильном формате). Ссылаться на [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) учебник по более подробно рассмотрим Настройка s GridView, интерфейс редактирования.

> [!NOTE]
> GridView, которые необходимо включить состояние представления s, так как исходные значения, передаваемые с GridView элемента управления SqlDataSource сохраняются в состоянии представления.


После внесения этих изменений в GridView, декларативная разметка GridView и SqlDataSource должен выглядеть следующим образом:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Чтобы увидеть оптимистическое управление параллелизмом в действии, откройте два окна браузера и загрузить `OptimisticConcurrency.aspx` страницы в обоих. Щелкните кнопки изменения для первого продукта в оба браузера. В один браузер названия продукта и нажмите кнопку обновления. Браузер выполнит обратную передачу и GridView будет возвращать его до редактирования режима, отображает имя продукта для записи только что изменили.

В окне второго обозревателя цену (оставив, название продукта как исходное значение) и нажмите кнопку обновления. При обратной передаче возвращает сетки в режим до редактирования, но не записи изменения цены. Второй обозреватель показано то же значение, что первый из них имя продукта с старая цена. Изменения, внесенные в окне второго обозревателя были потеряны. Кроме того изменения были потеряны довольно без вмешательства пользователя, так как не исключение или сообщение о том, что только что о возникновении одновременного нарушения.


[![Изменения в окне второго обозревателя автоматически удалены.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Рис. 7**: изменения в второй браузера окна были автоматически потеряны ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


Была причина, почему второй браузера s изменения не были зафиксированы, так как `UPDATE` инструкцию s `WHERE` предложение отфильтрованы все записи и поэтому не обработала все строки. Позвольте s рассмотрим `UPDATE` инструкцию еще раз:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

При втором обозревателе обновляет запись, исходное имя продукта, указанное в `WHERE` предложение t сопоставление с существующим именем продукта (так как он был изменен с помощью первого браузера). Таким образом, инструкция `[ProductName] = @original_ProductName` возвращает значение False и `UPDATE` не влияет на какие-либо записи.

> [!NOTE]
> Принцип работы DELETE таким же образом. С помощью двух окнах браузера откройте начните с редактирования данного продукта с одной и сохранять свои изменения. После сохранения изменений в один браузер, нажмите кнопку Delete на один продукт, в другой. Поскольку исходные значения Дон t связываете `DELETE` операторов `WHERE` предложение, удаление автоматически завершается сбоем.


С точки зрения конечного пользователя s во второе окно браузера после нажатия кнопки обновления сетки возвращает режиму до редактирования, но их изменения были потеряны. Тем не менее существует s не визуальную обратную связь, придерживайтесь t не подходит их изменения. В идеальном случае случае утери для одновременного нарушения изменения пользователя s мы d уведомлением и, возможно, сохранить сетке в режиме редактирования. Позвольте s рассмотрим, как для выполнения этой задачи.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Шаг 3: Определение, когда произошло нарушение параллелизма

Поскольку одновременного нарушения отклоняет изменения, внесенные одним, было бы неплохо для предупреждения пользователя при возникновении одновременного нарушения. Чтобы предупредить пользователя, позволяют s добавьте элемент управления Label Web в верхней части страницы с именем `ConcurrencyViolationMessage` которого `Text` свойство отображается следующее сообщение: предпринята попытка обновить или удалить запись, которая одновременно был обновлен другим пользователем. Можно просмотреть изменения другого пользователя и затем повторить обновление или удалить. Задать элемент управления "Метка" s `CssClass` свойства на "Предупреждение", который является классом CSS, определенного в `Styles.css` red, курсивом, полужирным шрифтом и большого шрифта, который отображает текст. Наконец, задайте для метки s `Visible` и `EnableViewState` свойства `false`. При этом будут скрыты за исключением только обратных передач, где мы прямо устанавливаем метку его `Visible` свойства `true`.


[![Добавьте элемент управления Label к странице для отображения предупреждения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Рис. 8**: добавьте элемент управления Label к странице для отображения предупреждения ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


При выполнении обновления или удаления, GridView s `RowUpdated` и `RowDeleted` обработчики событий срабатывать после его источник данных выполнил запрошенную update или delete. Мы можем определить, сколько строк участвовало операцию из этих обработчиков событий. Если ни одной строки были затронуты, нам требуется вывести `ConcurrencyViolationMessage` метки.

Создайте обработчик событий для обоих `RowUpdated` и `RowDeleted` события и добавьте следующий код:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

В обоих обработчиков событий мы проверяем `e.AffectedRows` свойства и, если он имеет значение 0, задайте `ConcurrencyViolationMessage` метка s `Visible` свойства `true`. В `RowUpdated` обработчик событий, мы также укажем среде GridView, чтобы оставаться в режиме редактирования, установив `KeepInEditMode` присваивается значение true. Таким образом, нам нужно привязать данные к сетке, чтобы другие данные пользователя s загружается в существующем интерфейсе правки. Это достигается посредством вызова GridView s `DataBind()` метод.

Как показано на рис. 9, с помощью этих двух обработчиков выросли сообщение отображается при каждом возникновении одновременного нарушения.


[![При возникновении одновременного нарушения отображается сообщение](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Рис. 9**: сообщение при возникновении одновременного нарушения ([Просмотр полноразмерного изображения](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Сводка

При создании веб-приложение, где несколько, одновременных пользователей может редактировать те же данные, важно рассмотреть варианты управления параллелизмом. По умолчанию данными ASP.NET, которые веб-элементов управления и элементов управления источниками данных не применяют любого элемента управления параллелизмом. Как мы видели в этом учебнике, реализация управления оптимистичным параллелизмом с помощью элемента управления SqlDataSource относительно быстро и легко. Элемента управления SqlDataSource обрабатывает большую часть подготовительная работа для вашей Добавление дополнить `WHERE` предложения, чтобы автоматически сформированного `UPDATE` и `DELETE` но инструкции являются несколько тонкостей в обработке `NULL` значения столбцов, как описано в Для правильной обработки `NULL` значения раздела.

Этот учебник завершает изучение элемента управления SqlDataSource. Руководств по оставшиеся вернется к работе с данными, используя элемент управления ObjectDataSource и многоуровневые архитектуры.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [Вперед](querying-data-with-the-sqldatasource-control-vb.md)
