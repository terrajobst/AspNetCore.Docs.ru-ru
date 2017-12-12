---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: "Настройка DataList Правка интерфейса (Visual Basic) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы создадим богатый интерфейс редактирования для DataList, содержащий элементами управления DropDownList и флажок."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: bd31c18b9fced12feeda8ca8dea7dace0ef63573
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Настройка интерфейса редактирования DataList (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) или [скачать PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> В этом учебнике мы создадим богатый интерфейс редактирования для DataList, содержащий элементами управления DropDownList и флажок.


## <a name="introduction"></a>Вступление

Разметка и веб-элементов управления в DataList s `EditItemTemplate` определить свой интерфейс для редактирования. Во всех редактируемых DataList примерах мы хранять освещенные к этому моменту, редактируемый интерфейс были состоит из текстового поля веб-элементов управления. В [предыдущего учебника](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) мы улучшить взаимодействие с пользователем во время редактирования, добавление элементов управления проверки.

`EditItemTemplate` Можно дополнительно расширить для включения веб-элементы управления, отличные от текстового поля, такие как календари элементами управления DropDownList, RadioButtonLists и т. д. Как и в текстовые поля, когда настройка интерфейса редактирования для включения других веб-элементов управления, используют следующие действия:

1. Добавить веб-элемент управления для `EditItemTemplate`.
2. Присваиваемое значение соответствующего поля данных соответствующим свойством, используйте синтаксис привязки данных.
3. В `UpdateCommand` обработчика событий, программный доступ к веб-сайте контроля значения и передайте его в соответствующий метод уровень бизнес-ЛОГИКИ.

В этом учебнике мы создадим богатый интерфейс редактирования для DataList, содержащий элементами управления DropDownList и флажок. В частности, мы создадим DataList, приведены сведения о продукте и позволяет использовать название продукта s, поставщика, категорию и неподдерживаемые состояния обновления (см. рис. 1).


[![Интерфейс редактирования содержит текстовое поле, двумя элементами управления DropDownList и флажка](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Рис. 1**: интерфейс редактирования содержит текстовое поле, двумя элементами управления DropDownList и CheckBox ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Шаг 1: Отображение сведений о продукте

Прежде чем создавать интерфейс для редактирования s DataList, нужно сначала создать интерфейс только для чтения. Сначала откройте `CustomizedUI.aspx` страницу `EditDeleteDataList` папки и добавьте DataList страницу, установка в режиме конструктора его `ID` свойства `Products`. В смарт-теге DataList s создайте новый элемент управления ObjectDataSource. Назовите этот новый элемент управления ObjectDataSource `ProductsDataSource` и настройте его для получения данных из `ProductsBLL` класса s `GetProducts` метод. Как с редактируемой предыдущих занятий DataList, будет обновлена сведения s продукта, перейдите непосредственно на уровне бизнес-логики. Соответственно задать раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет).


[![Задать раскрывающиеся списки UPDATE, INSERT и DELETE вкладок (нет)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**На рисунке 2**: установить обновления, вставки и удаления вкладок раскрывающихся списков (нет) ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


После настройки ObjectDataSource, Visual Studio создаст по умолчанию `ItemTemplate` DataList, содержащий имя и значение для каждого из полей данных результирующего набора. Изменить `ItemTemplate` так, чтобы название продукта в списки шаблон `<h4>` наряду с имя категории, имя поставщика, цены и состояния больше не поддерживаются. Кроме того, добавьте кнопку "Изменить", выбрав его `CommandName` свойству присвоено значение Edit. Декларативная разметка для моей `ItemTemplate` ниже:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Располагает выше разметки сведения продукта с помощью &lt;h4&gt; заголовок для имени продукта s и четырех столбцов `<table>` для остальных полей. `ProductPropertyLabel` И `ProductPropertyValue` классы CSS, определенные в `Styles.css`, обсуждались в предыдущих учебниках. На рисунке 3 показана ход работы при просмотре через браузер.


[![Отображается имя, поставщик, категория, неподдерживаемые состояния и цены каждого продукта](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Рис. 3**: отображается Name, поставщик, категория, состояние прекращена и цены каждого продукта ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Шаг 2: Добавление веб-элементы управления редактирования интерфейса

Первым шагом при создании настраиваемого DataList интерфейс редактирования заключается в добавлении необходимых веб-элементы управления для `EditItemTemplate`. В частности мы должны DropDownList для категории, другой для поставщика, а флажок для состояния больше не поддерживаются. Так как цена продукта s не редактируемые в этом примере, мы продолжаем, отображение с помощью веб-управления Label.

Настройка интерфейса редактирования, щелкните ссылку Изменить шаблоны из DataList s смарт-тег и выберите `EditItemTemplate` параметр из раскрывающегося списка. Добавить DropDownList для `EditItemTemplate` и задайте его `ID` для `Categories`.


[![Добавить DropDownList для категорий](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Рис. 4**: Добавление DropDownList для категорий ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Затем в DropDownList s смарт-теге, выберите параметр Выбор источника данных и создать новый элемент управления ObjectDataSource с именем `CategoriesDataSource`. Настройте этот элемент управления ObjectDataSource `CategoriesBLL` класса s `GetCategories()` метод (см. рис. 5). Далее DropDownList s мастер настройки источника данных предлагает для полей данных для каждого `ListItem` s `Text` и `Value` свойства. Отображение DropDownList имеют `CategoryName` поля данных и использование `CategoryID` как значение, как показано на рис. 6.


[![Создать новый элемент управления ObjectDataSource CategoriesDataSource с именем](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Рис. 5**: создать новый именованный ObjectDataSource `CategoriesDataSource` ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Настройка отображения s DropDownList и значение поля](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Рис. 6**: Настройка DropDownList s отображения и значение поля ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Повторите этот ряд действий по созданию DropDownList для поставщиков. Задать `ID` для этого DropDownList для `Suppliers` и назовите его ObjectDataSource `SuppliersDataSource`.

После добавления двумя элементами управления DropDownList, добавьте флажок неподдерживаемые состояния и текстовое поле для имени продукта s. Задать `ID` для CheckBox и TextBox для `Discontinued` и `ProductName`соответственно. Добавьте RequiredFieldValidator, чтобы убедиться, что пользователь предоставляет значение для имени продукта s.

И, наконец добавьте кнопки обновления и "Отмена". Помните, что для этих двух кнопок крайне важно, их `CommandName` свойства устанавливаются для обновления и отменить, соответственно.

Вы можете компоновать интерфейс редактирования, однако вам нравится. Я хранить решено использовать те же четыре столбца `<table>` показан макет из интерфейса только для чтения, как снимок экрана и следующий синтаксис:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Интерфейс редактирования находится упорядочивается как интерфейс только для чтения](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Рис. 7**: интерфейс редактирования находится упорядочивается как интерфейс только для чтения ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Шаг 3: Создание EditCommand и обработчики событий CancelCommand

В настоящее время нет синтаксис без привязки к данным в `EditItemTemplate` (за исключением `UnitPriceLabel`, который копируется по открытым текстом из `ItemTemplate`). Моментально будет добавлен синтаксис привязки данных, но первый позволяют s Создание обработчиков событий для DataList s `EditCommand` и `CancelCommand` события. Помните, что ответственность за `EditCommand` обработчик событий — для подготовки к просмотру интерфейс редактирования для элемента DataList была нажата, кнопка "Изменить", тогда как `CancelCommand` задание s — Чтобы вернуться в состояние до редактирования DataList.

Создайте эти два обработчика событий и их используйте следующий код:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

С этими двух обработчиков события в месте, нажав кнопку «Изменить» отображает интерфейс редактирования и нажав кнопку "Отмена" возвращает элемент в режим только для чтения. На рисунке 8 показана DataList после для s Креольская огненная смесь была нажата кнопка "Изменить". Поскольку мы хранять еще Добавление любой синтаксис привязки данных к интерфейсу редактирования `ProductName` текстовое поле не заполнено, `Discontinued` флажок и первые выбираются в `Categories` и `Suppliers` элементами управления DropDownList.


[![Щелкнув отображает кнопку Правка интерфейс редактирования](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Рис. 8**: нажмите кнопку «Изменить» отображает интерфейс редактирования ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Шаг 4: Добавление синтаксис привязки данных к интерфейсу редактирования

Чтобы отобразить текущие значения s продукта интерфейс редактирования, необходимо использовать синтаксис привязки данных для назначения значения полей данных в соответствующие значения веб-элемента управления. Синтаксис привязки данных может применяться в конструкторе, перейдя на экран Редактирование шаблонов и выбрав ссылку изменить привязки к данным из веб-элементы управления смарт-тегов. Кроме того можно добавить синтаксис привязки данных непосредственно в декларативной разметкой.

Назначить `ProductName` значение поля данных `ProductName` TextBox s `Text` свойства `CategoryID` и `SupplierID` значения для полей данных `Categories` и `Suppliers` элементами управления DropDownList `SelectedValue` свойства и `Discontinued` значение поля данных `Discontinued` флажок s `Checked` свойство. После внесения этих изменений в конструкторе или напрямую через декларативная разметка возвратиться к странице с помощью браузера и нажмите кнопку редактирования для s Креольская Gumbo Mix. Как показано на рис. 9, синтаксис привязки данных добавил текущие значения в текстовое поле, элементами управления DropDownList и флажок.


[![Щелкнув отображает кнопку Правка интерфейс редактирования](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Рис. 9**: нажмите кнопку «Изменить» отображает интерфейс редактирования ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Шаг 5: Сохранение изменений s пользователя в обработчике событий UpdateCommand

Когда пользователь редактирует продукта и нажимает кнопку обновления, обратная передача и DataList s `UpdateCommand` вызывается событие. В случае обработчик, мы должны считывать значения из веб-элементов управления в `EditItemTemplate` и интерфейса с помощью МЕТОДА для обновления продукта в базе данных. Как мы хранить в предыдущих учебниках `ProductID` обновленного продукта доступен с помощью `DataKeys` коллекции. Поля, введенные пользователем, осуществляется программного обращения к веб-элементов управления с помощью `FindControl("controlID")`, как показано в следующем коде:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Код запускается по журналу `Page.IsValid` свойство, чтобы обеспечить допустимость всех элементов управления проверкой на странице. Если `Page.IsValid` — `True`, продукта s `ProductID` значение, считанное из `DataKeys` коллекции и ввод данных, веб-элементов управления в `EditItemTemplate` ссылаются программными средствами. Далее считывания значений из этих веб-элементов управления в переменные, которые затем передаются в соответствующие `UpdateProduct` перегрузки. После обновления данных, DataList возвращается в состояние до редактирования.

> [!NOTE]
> Я хранить опущен логика, добавленная в, обрабатывающего [обработки уровень бизнес-ЛОГИКИ и исключения уровня DAL](handling-bll-and-dal-level-exceptions-vb.md) учебника для сохранения согласованности в коде и в этом примере с фокусом ввода. В качестве упражнения эти функции можно добавьте после завершения этого учебника.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Шаг 6: Обработка NULL CategoryID и SupplierID значения

База данных "Борей" позволяет для `NULL` значения для `Products` таблицу s `CategoryID` и `SupplierID` столбцов. Однако в настоящее время размещения нашей редактирования t интерфейс `NULL` значения. При попытке изменить продукта, который имеет `NULL` значение либо для его `CategoryID` или `SupplierID` столбцы, мы получаем `ArgumentOutOfRangeException` с сообщением об ошибке аналогично: *SelectedValue, который является недопустимым, так как она не имеет «Категории» не существует в списке элементов.* Кроме того, существует s в настоящее время изменить категории продукта s или поставщиком значение отличным от`NULL` значение `NULL` один.

Для поддержки `NULL` значения для категории и поставщика элементами управления DropDownList, нам нужно добавить дополнительный `ListItem`. Я хранить решено использовать (нет) в качестве `Text` значение для этого `ListItem`, но предпочитая d можно изменить на другое (например, пустая строка). Наконец, не забудьте установить элементами управления DropDownList для `AppendDataBoundItems` для `True`; Если вы забыли сделать это, по категориям и поставщики, привязанный к DropDownList перезапишет статически добавляется `ListItem`.

После внесения этих изменений разметки элементами управления DropDownList в DataList s `EditItemTemplate` должен выглядеть следующим образом:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Статические `ListItem` s могут добавляться в DropDownList в конструкторе или напрямую с помощью декларативного синтаксиса. При добавлении элемент DropDownList для представления базы данных `NULL` значений, не забудьте добавить `ListItem` посредством декларативного синтаксиса. При использовании `ListItem` редактора коллекции в конструкторе, будут исключаться созданный декларативный синтаксис `Value` параметр полностью, если назначенный пустые строки, создавая декларативной разметкой, например: `<asp:ListItem>(None)</asp:ListItem>`. Хотя это может показаться безопасным, отсутствующего `Value` вызывает DropDownList для использования `Text` значение свойства на его месте. Что означает, что если это `NULL` `ListItem` — флажок установлен, значение (нет) будет произведена попытка присвоить полю данных продукта (`CategoryID` или `SupplierID`, в этом учебнике), что приведет к возникновению исключения. После явного задания `Value=""`, `NULL` в продукт будет присвоено значение поля данных, когда `NULL` `ListItem` выбран.


Теперь пора просмотрите ход работы через браузер. При изменении продукта, обратите внимание, что `Categories` и `Suppliers` элементами управления DropDownList оба имеют (нет) параметр в начале DropDownList.


[![Категории и элементами управления DropDownList поставщики включают (None) параметр](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Рис. 10**: `Categories` и `Suppliers` элементами управления DropDownList включают (None) параметра ([Просмотр полноразмерное изображение](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Чтобы сохранить (нет) параметр как база данных `NULL` значение, необходимо вернуться к `UpdateCommand` обработчика событий. Изменение `categoryIDValue` и `supplierIDValue` переменных nullable целых чисел и назначить их значение отличное от `Nothing` только если DropDownList s `SelectedValue` не является пустой строкой:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Благодаря этому изменению значение `Nothing` будет передан в `UpdateProduct` параметр метода уровень бизнес-ЛОГИКИ, если пользователь выбрал (нет) из любой из раскрывающихся списков, которые соответствует `NULL` значение базы данных.

## <a name="summary"></a>Сводка

В этом учебнике мы узнали, как создавать более сложные редактирования интерфейс DataList и включены три различных входных веб-элементы управления TextBox, двумя элементами управления DropDownList и флажок, а также проверяющие элементы управления. При построении интерфейс редактирования, шаги одинаковы, независимо от веб-элементов управления, используемый: начать с добавления веб-элементы управления DataList s `EditItemTemplate`; используйте синтаксис привязки данных для назначения соответствующих веб-службы с соответствующими значениями полей данных свойства элемента управления; и в `UpdateCommand` обработчик событий программного доступа к веб-элементов управления и их соответствующие свойства, передайте их значения в МЕТОДА.

При создании интерфейса редактирования, является ли он s, состоящие из только текстовые поля или коллекцию различных веб-элементов управления, убедитесь, что для правильной обработки базы данных `NULL` значения. При учете `NULL` s, крайне важно вы не только правильного отображения существующего `NULL` значение в интерфейс редактирования, но также, которые предоставляют средства для классификации и пометки значения как `NULL`. Для элементами управления DropDownList в DataLists, это обычно означает Добавление статического `ListItem` которого `Value` явным образом задано равным пустой строке (`Value=""`) и добавив код, чтобы `UpdateCommand` обработчик событий для определения того, является ли `NULL``ListItem` был выбран.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Патерсона Деннис, Дэвид Suru и Рэнди Шмидт. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
