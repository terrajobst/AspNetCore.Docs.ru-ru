---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: "Обновление и удаление существующих двоичных данных (C#) | Документы Microsoft"
author: rick-anderson
description: "В предыдущих учебниках мы узнали, как элемент управления GridView упрощает изменение и удаление текстовых данных. В этом учебнике мы видим, как элемент управления GridView также устанавливают..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: f2fca1e91720fba0215e12b1a1894a3a31e86b5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="updating-and-deleting-existing-binary-data-c"></a>Обновление и удаление существующих двоичных данных (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) или [скачать PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> В предыдущих учебниках мы узнали, как элемент управления GridView упрощает изменение и удаление текстовых данных. В этом учебнике мы узнаем, как элемент управления GridView также делает возможным изменение и удаление двоичных данных, двоичные данные сохранены в базе данных или хранящихся в файловой системе.


## <a name="introduction"></a>Вступление

За последние три учебника мы хранять добавлен бит функциональные возможности для работы с двоичными данными. Мы запустили путем добавления `BrochurePath` столбец `Categories` таблицы и соответствующим образом обновлены архитектуры. Мы также добавили уровня доступа к данным и бизнес-логики методы для работы с существующие таблицы s категории `Picture` столбец, который содержит двоичные s содержимого файла изображения. Мы создали веб-страницы для двоичных данных в элементе управления GridView предоставить ссылку для загрузки для брошюр, прокрутки рисунка категории s `<img>` элемент и добавили DetailsView, чтобы пользователи могли добавить новую категорию и отправить свои данные брошюр и изображения.

Для реализации остается возможность изменять и удалять существующие категории, которые мы будем выполнять в этом учебнике с помощью GridView s встроенного редактирования и удаления компонентов. При редактировании категорию, пользователь будет при необходимости загрузить новое изображение или относятся к категории продолжать использовать существующий. Для брошюр они могут выбрать использование существующей брошюр, отправить новый брошюр или указать, что категория больше не имеет брошюр, связанные с ним. S позволяют начать работу!

## <a name="step-1-updating-the-data-access-layer"></a>Шаг 1: Обновление уровня доступа к данным

DAL создан `Insert`, `Update`, и `Delete` методы, но эти методы были созданы на основе `CategoriesTableAdapter` основной запрос s, которая не включает `Picture` столбца. Таким образом `Insert` и `Update` методы не следует включать параметры для указания двоичные данные для изображения категории s. Как мы делали [предыдущего учебника](including-a-file-upload-option-when-adding-a-new-record-cs.md), нам нужно создать новый метод адаптера таблицы для обновления `Categories` таблице при указании двоичных данных.

Откройте типизированного набора данных и в режиме конструктора щелкните правой кнопкой мыши `CategoriesTableAdapter` s заголовка и выберите Добавить запрос в контекстном меню для launche мастер настройки запроса адаптера таблицы. Этот мастер с помощью вопроса, как запроса адаптера таблицы следует обращаться к базе данных. Выберите использовать инструкции SQL и нажмите кнопку Далее. Следующий шаг предлагаемые для типа запроса. Поскольку мы повторно создать запрос, чтобы добавить новую запись для `Categories` таблицы, выберите обновление и нажмите кнопку Далее.


[![Выберите вариант обновления](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Рис. 1**: выберите параметр обновления ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


Теперь нам нужно указать `UPDATE` инструкции SQL. Мастер автоматически предлагает `UPDATE` инструкции, соответствующий s основного запроса адаптера таблицы (который обновляет `CategoryName`, `Description`, и `BrochurePath` значения). Измените инструкцию, чтобы `Picture` включен столбец вместе с `@Picture` параметр следующим образом:


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

На последнем экране мастера появляется запрос на имя нового метода адаптера таблицы. Введите `UpdateWithPicture` и нажмите кнопку Готово.


[![Имя нового UpdateWithPicture метод адаптера таблицы](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**На рисунке 2**: имя нового метода адаптера таблицы `UpdateWithPicture` ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Шаг 2: Добавление методов бизнес-логики слоя

Наряду с обновлением DAL, необходимо обновить уровень бизнес-ЛОГИКИ для включения методов для обновления и удаления категории. Это методы, которые будут вызываться из уровня представления.

Для удаления категории, можно использовать `CategoriesTableAdapter` s автоматически созданный `Delete` метод. Добавьте следующий метод `CategoriesBLL` класса:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

В этом учебнике позволяют s создайте два метода для обновления категории - та, которая ожидает рисунок двоичных данных и вызывает `UpdateWithPicture` метод уже добавлен к `CategoriesTableAdapter` , а другая, которая принимает только `CategoryName`, `Description`и `BrochurePath`значения и использует `CategoriesTableAdapter` класс создан s `Update` инструкции. Обоснование с помощью двух методов является, в некоторых случаях пользователь может потребоваться обновление рисунка s категории вместе с его другие поля, в которых случае пользователю необходимо отправить новый рисунок. Отправленный рисунок s двоичные данные затем могут использоваться в `UPDATE` инструкции. В других случаях пользователю только могут быть заинтересованы в обновлении, например, имя и описание. Но если `UPDATE` инструкция ожидает двоичные данные для `Picture` также столбец, а затем мы d необходимо указать подобных сведений. Это потребует обращения к базе данных, чтобы вернуть данные рисунка изменяемую запись. Таким образом, мы хотим два `UPDATE` методы. Определяет уровень бизнес-логики один из них зависимости от того, предоставляется ли изображение данных при обновлении категории.

Для этого добавьте два метода для `CategoriesBLL` класса, оба с именем `UpdateCategory`. Первый должен принимать три `string` s, `byte` массива и `int` качестве входных данных параметры; второе, просто три `string` s и `int`. `string` Входные параметры являются категории s имя, описание и путь к файлу брошюр, `byte` массив — для двоичного содержимого рисунка категории s и `int` идентифицирует `CategoryID` записи для обновления. Обратите внимание, что первая перегрузка вызывает секунды, если переданный `byte` массив `null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Шаг 3: Копирование через функцию вставки и представления

В [предыдущего учебника](including-a-file-upload-option-when-adding-a-new-record-cs.md) мы создали страницу с именем `UploadInDetailsView.aspx` , перечислены все категории в элементе управления GridView и предоставляемые DetailsView, чтобы добавить новые категории в системе. В этом учебнике мы расширить GridView, чтобы включить редактирование и удаление поддержки. Вместо того, чтобы продолжить работать с `UploadInDetailsView.aspx`, позволяют s вместо поместить этот учебник s изменения в `UpdatingAndDeleting.aspx` страницы из той же папки `~/BinaryData`. Скопируйте и вставьте декларативная разметка и код из `UploadInDetailsView.aspx` для `UpdatingAndDeleting.aspx`.

Сначала откройте `UploadInDetailsView.aspx` страницы. Скопируйте все декларативного синтаксиса в `<asp:Content>` элемента, как показано на рис. 3. Затем откройте `UpdatingAndDeleting.aspx` и вставьте этот разметка в рамках его `<asp:Content>` элемента. Аналогичным образом, скопируйте код из `UploadInDetailsView.aspx` странице кода класса s `UpdatingAndDeleting.aspx`.


[![Скопируйте декларативная разметка из UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Рис. 3**: скопируйте декларативная разметка из `UploadInDetailsView.aspx` ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


После копирования через декларативная разметка и код, посетите `UpdatingAndDeleting.aspx`. Вы увидите выходные данные и имеют одинаковые взаимодействие с пользователем как в случае с `UploadInDetailsView.aspx` страницы из предыдущего учебника.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Шаг 4: Добавление, удаление поддержки ObjectDataSource и GridView

Как уже говорилось в [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) учебнике GridView предоставляет встроенные возможности удаления, и эти возможности может быть включена в деления флажок, если основной сетки s источник данных поддерживает удаление. В настоящее время ObjectDataSource GridView привязан к (`CategoriesDataSource`) не поддерживает удаление.

Чтобы исправить это, щелкните параметр настройки источника данных из ObjectDataSource s смарт-тег для запуска мастера. Первый экран показывает, что элемент управления ObjectDataSource настроен для работы с `CategoriesBLL` класса. Нажмите Далее. В настоящее время только ObjectDataSource s `InsertMethod` и `SelectMethod` указаны свойства. Тем не менее, мастер автоматически заполняется в раскрывающихся списках на вкладках UPDATE и DELETE с `UpdateCategory` и `DeleteCategory` методов, соответственно. Это, поскольку в `CategoriesBLL` класса, помеченного как эти методы, с помощью мы `DataObjectMethodAttribute` как методы по умолчанию для обновления и удаления.

На данном этапе значение раскрывающегося списка обновления вкладки s (нет), но оставьте значение раскрывающегося списка DELETE вкладки s `DeleteCategory`. Мы вернемся к этому мастеру в шаге 6, чтобы добавить поддержку обновления.


[![Настройка ObjectDataSource можно использовать метод DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Рис. 4**: Настройка ObjectDataSource для использования `DeleteCategory` метод ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> После завершения работы мастера, Visual Studio может потребоваться, если вы хотите обновить поля и ключи, что приведет к повторному созданию данных Web управляет поля. Выберите Нет, поскольку Выбор Да перезапишет все настройки полей, возможно, были внесены.


ObjectDataSource будут включать значение для его `DeleteMethod` свойства, а также `DeleteParameter`. Помните, что при использовании мастера для указания методов, Visual Studio устанавливает ObjectDataSource s `OldValuesParameterFormatString` свойства `original_{0}`, что вызывает проблемы с обновлением и удалите вызовы методов. Таким образом, либо полностью очистить это свойство или сбросить до значений по умолчанию `{0}`. Если вам необходимо обновить память на это свойство ObjectDataSource, см. раздел [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) учебника.

После завершения работы мастера и исправления `OldValuesParameterFormatString`, ObjectDataSource s должна выглядеть примерно следующим образом:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

После настройки ObjectDataSource, добавления Удаление возможностей в GridView, установив флажок Разрешить удаление смарт-теге s GridView. Это добавит CommandField GridView которого `ShowDeleteButton` свойству `true`.


[![Включите поддержку для удаления в GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Рис. 5**: Включение поддержки удаление в GridView ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


Теперь пора проверить функцию удаления. Нет внешнего ключа между `Products` таблицу s `CategoryID` и `Categories` таблицу s `CategoryID`, поэтому вы получите нарушение ограничения внешнего ключа, если вы пытаетесь удалить любой из первых восьми категорий. Протестировать эти функции out, добавьте новую категорию, предоставляя брошюр и изображения. Мои категории тестов, показанный на рисунке 6 включает брошюр тестовый файл с именем `Test.pdf` и пробного рисунка. На рисунке 7 показано GridView, после добавления категории тестов.


[![Добавить категорию с брошюр и изображением](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Рис. 6**: добавить категорию с брошюр и Image ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![После вставки категории тестов, он отображается в GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Рис. 7**: после вставки категории тестов, он отображается в GridView ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


В Visual Studio обновление обозревателя решений. Должно появиться в новом файле в `~/Brochures` папке `Test.pdf` (см. рис. 8).

Затем щелкните ссылку «Удалить» в строке категории тестов обратной передачи страницы и `CategoriesBLL` класса s `DeleteCategory` метод срабатывание. Это вызовет DAL s `Delete` метода, вынуждая соответствующего `DELETE` инструкции, отправляемые в базу данных. Данных затем привязывается к GridView и разметку отправляется обратно клиенту с категории тестов больше нет.

Во время удаления рабочего процесса успешно удалена запись категории тестов из `Categories` таблицы, она не была удалена его брошюр файл из файловой системе сервера s веб. Обновление обозревателя решений, и вы увидите, что `Test.pdf` по-прежнему находится в `~/Brochures` папки.


![Файл Test.pdf не был удален из файловой системы Web Server s](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Рис. 8**: `Test.pdf` файл не был удален из файловой системы Web Server s


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Шаг 5: Удаление файла брошюр s удаленные категории

Один из недостатков для хранения двоичных данных, внешних по отношению к базе данных является, необходимо выполнить дополнительные действия для очистки этих файлов при удалении записи связанной базы данных. GridView и ObjectDataSource предоставляют события, которые вызывают срабатывание до и после выполнения команды удаления. Мы действительно нужны для создания обработчиков событий для событий до и после действия. Прежде чем `Categories` удаляется запись нам нужно определить пути к файлу s PDF, но мы не хотите t требуется удалить в формате PDF, перед удалением категории в случае некоторые исключения, а категории не удаляется.

GridView s [ `RowDeleting` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) активируется до команды удаления ObjectDataSource s был вызван, пока его [ `RowDeleted` событие](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) срабатывает после. Создание обработчиков событий для этих двух событий, используя следующий код:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

В `RowDeleting` обработчик событий `CategoryID` строки удаляются извлечен из GridView s `DataKeys` коллекции, к которому можно получить в этом обработчике событий через `e.Keys` коллекции. Далее, `CategoriesBLL` класса s `GetCategoryByCategoryID(categoryID)` вызывается для возвращения сведений о записи удаляются. Если возвращенный `CategoriesDataRow` объект имеет значение, отличное от`NULL``BrochurePath` , то он хранится в переменной страницы `deletedCategorysPdfPath` , чтобы этот файл можно удалить в `RowDeleted` обработчика событий.

> [!NOTE]
> Вместо передачи `BrochurePath` сведений о `Categories` записи удаляются в `RowDeleting` обработчик событий может также добавлена `BrochurePath` к GridView s `DataKeyNames` свойства и получить доступ к значения записи s через `e.Keys` коллекции. Таким образом будет немного увеличить размер состояния представления s GridView, но следует уменьшить объем кода, необходимого и сохранить маршрут в базе данных.


После базовой команды delete s был вызван, ObjectDataSource GridView s `RowDeleted` вызывается обработчик события. Если не было исключений в удаления данных, а значение для `deletedCategorysPdfPath`, а затем PDF-ФАЙЛ удаляется из файловой системы. Обратите внимание, что этот дополнительный код не нужен для очистки категории s двоичные данные, связанные с его рисунок. S, так как данные изображения хранятся непосредственно в базе данных, поэтому удаление `Categories` строки также удаляет категории s данных рисунка.

После добавления два обработчика событий, снова запустите этот тестовый случай. При удалении категории, его связанные PDF также удаляется.

Обновление существующих двоичных данных записи, связанные предоставляет некоторые интересные задачи. В оставшейся части этого учебника углубляется в добавление возможностей обновления брошюр и изображения. Шаг 6 рассматриваются способы обновления данных брошюр пока просматривает обновление рисунок шаг 7.

## <a name="step-6-updating-a-category-s-brochure"></a>Шаг 6: Обновление брошюр s категории

Как было сказано в [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) учебнике GridView обеспечивает встроенную поддержку редактирования уровня строк, который может быть реализован деления флажок, если базовым источником данных правильно настроен. В настоящее время `CategoriesDataSource` ObjectDataSource еще не настроен для включения обновление поддержки, поэтому s позволяют добавить в.

Щелкните ссылку настроить источник данных с помощью мастера s ObjectDataSource и перейдите ко второму шагу. Из-за `DataObjectMethodAttribute` используется в `CategoriesBLL`, раскрывающегося списка обновления должны автоматически заполняться `UpdateCategory` перегрузку, которая принимает четыре входных параметра (для всех столбцов, но `Picture`). Измените, чтобы он использовал перегруженный с пятью параметрами.


[![Настройка ObjectDataSource использовать метод UpdateCategory, которое включает параметр для изображения](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Рис. 9**: Настройка ObjectDataSource для использования `UpdateCategory` метод, который включает параметр для `Picture` ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


ObjectDataSource будут включать значение для его `UpdateMethod` свойства, а также соответствующий `UpdateParameter` s. Как указано в шаге 4, Visual Studio устанавливает ObjectDataSource s `OldValuesParameterFormatString` свойства `original_{0}` при использовании мастера настройки источника данных. Это будет привести к проблемам с обновлением и удалите вызовы методов. Таким образом, либо полностью очистить это свойство или сбросить до значений по умолчанию `{0}`.

После завершения работы мастера и исправления `OldValuesParameterFormatString`, ObjectDataSource s должна выглядеть следующим образом:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Чтобы включить GridView s встроенные возможности редактирования, установите флажок Разрешить изменение смарт-теге s GridView. Это задаст CommandField s `ShowEditButton` свойства `true`, полученный в добавлении кнопка "Изменить" (и кнопки "обновления" и "Отмена" для изменяемой строки).


[![Настройка поддерживают редактирование GridView](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Рис. 10**: Настройка GridView поддерживают редактирование ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


Перейдите на страницу с помощью браузера и выберите один из строки s редактирование кнопок. `CategoryName` И `Description` стояли подготавливаются к просмотру как текстовые поля. `BrochurePath` Отсутствуют TemplateField `EditItemTemplate`, поэтому он продолжает отображать его `ItemTemplate` ссылку брошюр. `Picture` ImageField готовится к просмотру как элемент управления TextBox, `Text` присваивается значение ImageField s `DataImageUrlField` значение в этом случае `CategoryID`.


[![GridView отсутствует интерфейс редактирования для BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Рис. 11**: GridView отсутствует интерфейс редактирования для `BrochurePath` ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Настройка`BrochurePath`интерфейс редактирования s

Необходимо создать интерфейс для редактирования `BrochurePath` TemplateField, который пользователь может либо:

- Оставьте брошюр категории s как-,
- Обновить категории s брошюр передав новый брошюр, или
- Полностью удалите брошюр категории s (в случае, категорию, больше не имеет связанного брошюр).

Необходимо также обновить `Picture` интерфейс редактирования ImageField s, но у нас получите это в шаге 7.

В смарт-теге s GridView, щелкните ссылку Изменить шаблоны и выберите `BrochurePath` TemplateField s `EditItemTemplate` из раскрывающегося списка. Добавить RadioButtonList веб-элемент управления к этому шаблону, установка его `ID` свойства `BrochureOptions` и его `AutoPostBack` свойства `true`. В окне свойств нажмите кнопку с многоточием в `Items` свойство, которое появится `ListItem` редактор коллекции. Добавьте следующие три варианта с `Value` s 1, 2 и 3 соответственно:

- Использовать текущий брошюр
- Удалите текущий брошюр
- Отправьте новый брошюр

Устанавливает для первых `ListItem` s `Selected` свойства `true`.


![Добавление трех элементов ListItem RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Рис. 12**: добавьте три `ListItem` s, чтобы RadioButtonList


Под RadioButtonList, добавьте элемент управления FileUpload `BrochureUpload`. Задайте его `Visible` свойства `false`.


[![Добавление EditItemTemplate RadioButtonList и элемента управления FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Рис. 13**: Добавление RadioButtonList и управления FileUpload `EditItemTemplate` ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


Это RadioButtonList предоставляет три варианта для пользователя. Идея является отображение элемента управления FileUpload только в том случае, если последний параметр передачи нового брошюр, выбран. Чтобы сделать это, создайте обработчик событий для RadioButtonList s `SelectedIndexChanged` событий и добавьте следующий код:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Поскольку элементы управления RadioButtonList и FileUpload внутри шаблона, нужно написать большой объем кода для программного доступа к эти элементы управления. `SelectedIndexChanged` Обработчик событий передается ссылка RadioButtonList в `sender` входного параметра. Для элемента управления FileUpload, нам нужно получить родительский s RadioButtonList управления и использования `FindControl("controlID")` метод оттуда. После получения ссылку на элементы управления RadioButtonList и FileUpload FileUpload управлять s `Visible` свойству `true` только если RadioButtonList s `SelectedValue` равняется 3, который является `Value` для нового брошюр передачи `ListItem`.

Этот код в месте занять некоторое время для тестирования интерфейса редактирования. Нажмите кнопку "Изменить" для строки. Изначально брошюр текущего используйте параметр должен быть выбран. Изменение выбранного индекса вызывает обратную передачу. Если третий параметр выбран, элемент управления FileUpload отображается, в противном случае он скрыт. На рисунке 14 показана интерфейс редактирования, когда сначала нажата кнопка "Изменить"; Рис. 15 показан интерфейс, после отправки нового брошюр вариант.


[![Изначально текущего использования брошюр выбран параметр](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Рис. 14**: поначалу использование текущего брошюр выбран параметр ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![Выбор элемента управления FileUpload брошюр передачи новый параметр отображает](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Рис. 15**: Выбор брошюр передачи новый параметр отображает элемент управления FileUpload ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Сохранение брошюр файла и обновление`BrochurePath`столбца

При нажатии кнопки обновления s GridView, его `RowUpdating` вызывается событие. ObjectDataSource вызывается s команды update, а затем GridView s `RowUpdated` вызывается событие. Как и удаление рабочего процесса, нам нужно создавать обработчики событий для обоих этих событий. В `RowUpdating` обработчика событий, необходимо определить, какое действие необходимо выполнить на основе `SelectedValue` из `BrochureOptions` RadioButtonList:

- Если `SelectedValue` -1, необходимо сохранить, используя те же `BrochurePath` параметр. Таким образом, нам нужно указать ObjectDataSource s `brochurePath` параметр к существующему `BrochurePath` значение обновляемой записи. ObjectDataSource s `brochurePath` параметра можно задать с помощью `e.NewValues["brochurePath"] = value`.
- Если `SelectedValue` равно 2, то нам нужно выбрать запись s `BrochurePath` значение `NULL`. Это можно сделать, задав ObjectDataSource s `brochurePath` параметр `Nothing`, что приводит к базе данных `NULL` , используемых в `UPDATE` инструкции. Если имеется существующий файл брошюр, которое удаляется, необходимо удалить существующий файл. Однако нам нужен только в случае, если обновление завершается без возникновения исключения.
- Если `SelectedValue` равно 3, а затем мы хотим убедиться, что пользователь загрузил в PDF-файл и сохраните его в файловой системе и обновить запись s `BrochurePath` значение столбца. Кроме того Если имеется существующий файл брошюр, который заменяется, необходимо удалить предыдущий файл. Однако нам нужен только в случае, если обновление завершается без возникновения исключения.

Шаги, необходимые для выполнения при RadioButtonList s `SelectedValue` — 3 практически идентичны используемым DetailsView s `ItemInserting` обработчика событий. Этот обработчик событий выполняется при добавлении новой записи категории из элемента управления DetailsView, мы добавили в [с предыдущим учебником](including-a-file-upload-option-when-adding-a-new-record-cs.md). Таким образом он behooves нам рефакторинг этих функциональных возможностей системы в отдельные методы. В частности я перемещения общие функциональные возможности на два метода:

- `ProcessBrochureUpload(FileUpload, out bool)`принимает в качестве входных данных экземпляра элемента управления FileUpload и выходное значение типа Boolean, указывает ли операция удаления или изменения следует продолжить или если его следует отменить связи с ошибкой проверки. Этот метод возвращает путь к сохраненный файл или `null` Если файл не был сохранен.
- `DeleteRememberedBrochurePath`Удаляет файл, указанный в пути в переменную страницы `deletedCategorysPdfPath` Если `deletedCategorysPdfPath` не `null`.

Выполняет код для этих двух методов. Обратите внимание, сходство между `ProcessBrochureUpload` и DetailsView s `ItemInserting` обработчик событий из предыдущего учебника. В этом учебнике я обновили обработчики событий s DetailsView, чтобы использовать эти новые методы. Загрузите код, связанный с данным учебником, чтобы увидеть изменения в обработчики событий s DetailsView.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

GridView s `RowUpdating` и `RowUpdated` использовать обработчики событий `ProcessBrochureUpload` и `DeleteRememberedBrochurePath` методов, как показано в следующем коде:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Обратите внимание как `RowUpdating` обработчик событий использует ряд условных операторов для выполнения операции по `BrochureOptions` RadioButtonList s `SelectedValue` значение свойства.

Этот код в месте можно изменить категорию и осуществляет их использовать его текущего брошюр, использовать не брошюр или отправить новый. Продолжим и проверьте его работу. Установите точки останова в `RowUpdating` и `RowUpdated` обработчики событий, чтобы получить общее представление рабочего процесса.

## <a name="step-7-uploading-a-new-picture"></a>Шаг 7: Отправка нового рисунка

`Picture` ImageField s, изменив отображает интерфейс как текстовое поле заполняется значением из его `DataImageUrlField` свойство. Во время редактирования рабочего процесса, GridView передает параметр с именем параметра s ObjectDataSource значение ImageField s `DataImageUrlField` свойство и параметр s значение значением, введенным в текстовое поле в интерфейсе редактирования. Это подходит, когда изображение сохраняется как файл в файловой системе и `DataImageUrlField` содержит полный URL-адрес изображения. В такой ситуации интерфейс редактирования отображаются URL-адрес изображения s в текстовом поле, который пользователь может изменить и сохранены в базе данных. Разумеется, t интерфейс по умолчанию пользователи могут отправить новый образ, но позволяем изменяются URL-адрес изображения в зависимости от текущего значения. В этом учебнике, тем не менее, значение по умолчанию s ImageField интерфейс редактирования не достаточно поскольку `Picture` двоичные данные хранятся непосредственно в базе данных и `DataImageUrlField` содержится в свойстве просто `CategoryID`.

Чтобы лучше понять, что происходит в нашем руководстве при изменении пользователем строки с ImageField, рассмотрим следующий пример: пользователь не изменит строки с `CategoryID` 10, в результате чего `Picture` ImageField для подготовки к просмотру как элемент управления textbox с значение 10. Предположим, что пользователь изменяет значение в этом текстовом поле на 50 и нажатии кнопки "Обновить". Обратная передача и GridView вначале создается параметр с именем `CategoryID` со значением 50. Тем не менее прежде чем GridView отправляет этот параметр (и `CategoryName` и `Description` параметров), он добавляет в значения из `DataKeys` коллекции. Таким образом, он переопределяет `CategoryID` параметр с текущей основной строки s `CategoryID` значение 10. Иными словами, s ImageField, интерфейс редактирования не оказывает воздействия на редактирования рабочего процесса для этого учебника так как имена ImageField s `DataImageUrlField` свойство и сетки s `DataKey` значение — один в том же.

Хотя ImageField делает для отображения изображения на основе данных базы данных, мы не хотите t необходимо использовать текстовое поле в интерфейсе редактирования. Вместо этого мы хотим предложить элемента управления FileUpload, конечного пользователя можно использовать для изменения рисунка s категории. В отличие от `BrochurePath` значения для этих учебников мы решили хранить потребовать от каждой категории рисунка. Таким образом, мы не хотите необходимость позволить пользователю означает, что не связан рисунок пользователь может или загрузить новое изображение или оставьте текущее изображение как t-является.

Настройка интерфейса редактирования ImageField s, необходимо преобразовать его в TemplateField. В смарт-теге s GridView щелкните ссылку Изменить столбцы, выберите ImageField и щелкните преобразовать это поле в TemplateField ссылку.


![Преобразование ImageField в TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**На рисунке 16**: преобразование ImageField в TemplateField


Преобразование ImageField в TemplateField таким образом формирует TemplateField с двумя шаблонами. Как показано на следующем декларативного синтаксиса, `ItemTemplate` содержит изображение веб-элемент управления `ImageUrl` присваивается с помощью синтаксиса привязки данных в зависимости от ImageField s `DataImageUrlField` и `DataImageUrlFormatString` свойства. `EditItemTemplate` Содержит текстовое поле, `Text` свойство привязано к значения, указанного в `DataImageUrlField` свойство.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Необходимо обновить `EditItemTemplate` с помощью элемента управления FileUpload. Из GridView, щелкните смарт-тег s редактирование шаблонов связь, а затем выберите `Picture` TemplateField s `EditItemTemplate` из раскрывающегося списка. В шаблоне должны появиться удаления этого текстового поля. Затем перетащите элемент управления FileUpload из области элементов в шаблон, установка его `ID` для `PictureUpload`. Добавьте текст, чтобы изменить изображение категории s, укажите новое изображение. Чтобы сохранит рисунка категории s, оставьте поле пустым в шаблон.


[![Добавление элемента управления FileUpload EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Рисунок 17**: добавить элемент управления FileUpload `EditItemTemplate` ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


После настройки интерфейса правки, ход выполнения отображается в браузере. При просмотре строку в режиме только для чтения, как до, но кнопки "Изменить" отображает рисунок столбца в виде текста с помощью элемента управления FileUpload показано изображение категории s.


[![Интерфейс редактирования включает элемент управления FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**На рисунке 18**: редактирование интерфейса включает элемент управления FileUpload ([Просмотр полноразмерное изображение](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


Помните, что элемент управления ObjectDataSource настраивается для вызова `CategoriesBLL` класса s `UpdateCategory` метод, который принимает в качестве входных данных двоичные данные для изображения как `byte` массива. Если этот массив содержит `null` значение, однако альтернативного `UpdateCategory` вызывается перегрузка, какие проблемы `UPDATE` инструкции SQL, которая не изменяет `Picture` столбца, благодаря чему категории s текущий рисунок без изменений. Таким образом, в GridView s `RowUpdating` обработчик событий, нам необходимо программно ссылаться `PictureUpload` FileUpload управления и определить, если файл был загружен. Если один не был загружен, то мы делаем *не* необходимо указать значение для `picture` параметра. С другой стороны, если файл был отправлен на `PictureUpload` управления FileUpload, мы хотим убедиться, что это JPG-файл. Если он является, то мы можем отправить ее двоичное содержимое в элемент управления ObjectDataSource через `picture` параметра.

Как и с кодом, используемые на шаге 6, большая часть кода, необходимого уже существует в DetailsView s `ItemInserting` обработчика событий. Таким образом, я хранить рефакторинг общие функциональные возможности в новый метод `ValidPictureUpload`и обновить `ItemInserting` обработчика событий с помощью этого метода.

Добавьте следующий код в начало GridView s `RowUpdating` обработчика событий. Он s важные происхождения этот код перед кодом, который сохраняет файл брошюр, так как мы не хотите t будет сохранен в файловой системе сервера s веб брошюр, при отправке файла Недопустимое изображение.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

`ValidPictureUpload(FileUpload)` Метод принимает в элементе управления FileUpload как единственный входной параметр и проверяет расширение загруженного файла s, убедитесь, что загруженный файл JPG; он вызывается, только при отправке файла изображения. Если файл не загружен, то изображение параметр не задан и поэтому используется значение по умолчанию `null`. Если был загружен рисунок и `ValidPictureUpload` возвращает `true`, `picture` назначен параметр двоичные данные, переданные изображения; Если метод возвращает `false`, отмены рабочего процесса обновления и завершить работу обработчика событий.

`ValidPictureUpload(FileUpload)` Код метод, который были подвергнуты рефакторингу в DetailsView s `ItemInserting` событий, будет выглядеть следующим образом:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Шаг 8: Замените исходные изображения категорий JPGs

Помните, что исходные изображения восьми категорий являются файлы точечных рисунков, заключенное в заголовок OLE. Теперь, когда мы добавили возможность изменить существующую запись s картинку, пора замените JPGs эти битовые карты. Если вы хотите продолжать использовать текущий рисунки категории, их можно преобразовать в JPGs, выполнив следующие действия:

1. Сохранение растровых изображений на жестком диске. Посетите `UpdatingAndDeleting.aspx` страницы в браузере, так и для каждой из первых восьми категорий, щелкните правой кнопкой мыши на изображение и сохранить изображение.
2. Откройте изображение в редакторе изображений по выбору. Например можно использовать Microsoft Paint.
3. Сохраните растровое изображение в виде изображения JPG.
4. Обновление рисунка категории s через интерфейс редактирования с помощью JPG-файл.

После изменения категории и отправке изображения JPG, изображение будет не визуализации в браузере, так как `DisplayCategoryPicture.aspx` страницы удаление первых 78 байтов из изображений из первых восьми категорий. Исправить эту проблему путем удаления код, который выполняет удаление заголовка OLE. После этого, `DisplayCategoryPicture.aspx``Page_Load` обработчик событий должен иметь только следующий код:


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` s страниц, вставка и редактирование интерфейсы может использовать больше усилий. `CategoryName` И `Description` TemplateFields следует преобразовать стояли в DetailsView и GridView. Поскольку `CategoryName` не допускает `NULL` значения, должны быть добавлены RequiredFieldValidator. И `Description` текстовое поле, вероятно, должны быть преобразованы в многострочном текстовом окне. Я оставить эти внести Завершающие штрихи в качестве упражнения для вас.


## <a name="summary"></a>Сводка

Этот учебник завершает нашей рассмотрим работа с двоичными данными. В этом учебнике и предыдущих трех, мы узнали, как двоичные данные могут храниться в файловой системе или непосредственно в базе данных. Пользователь предоставляет двоичные данные в систему, выбрав файл из них жесткий диск и передать его на веб-сервер, где его можно хранить в файловой системе или вставлены в базу данных. ASP.NET 2.0 включает элемент управления FileUpload, который делает, предоставляя такой интерфейс так же легко, как перетаскивание. Тем не менее, как указано в [передачи файлов](uploading-files-cs.md) учебнике управления FileUpload только в том случае хорошо подходит для загрузки относительно небольшой файл, в идеале не превышая мегабайта. Мы также анализировать связывание загруженные данные с базовой моделью данных, а также изменение и удаление двоичных данных из существующих записей.

Наш следующий набор учебников рассматриваются различные методы кэширования. Кэширование позволяет повысить s приложения общую производительность извлечение результатов из ресурсоемких операций и сохранив их в расположении, которое может осуществляться быстрее.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было Мерфи Тереза д. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](including-a-file-upload-option-when-adding-a-new-record-cs.md)
[Вперед](uploading-files-vb.md)
