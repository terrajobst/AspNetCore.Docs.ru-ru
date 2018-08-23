---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Настройка элемента управления DataList редактирования интерфейса (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы создадим более богатый интерфейс редактирования элемента управления DataList, содержащий элементов управления DropDownList и флажок.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f7723895dd50b1923de49ca4a3a7055bbad5fe4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836052"
---
<a name="customizing-the-datalists-editing-interface-c"></a>Настройка интерфейса правки элемента управления DataList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) или [скачать PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> В этом руководстве мы создадим более богатый интерфейс редактирования элемента управления DataList, содержащий элементов управления DropDownList и флажок.


## <a name="introduction"></a>Вступление

Разметка и веб-элементов управления в элементе управления DataList s `EditItemTemplate` определяют его интерфейс редактирования. Во всех редактируемых элементов управления DataList примерах мы ve освещенные к этому моменту, изменяемый интерфейс, состояли из текстового поля веб-элементов управления. В [предыдущем учебном курсе](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) улучшено взаимодействие с пользователем во время редактирования путем добавления элементов управления проверки.

`EditItemTemplate` Может быть развернут для включения веб-элементы управления, отличного от текстового поля, например элементов управления DropDownList, RadioButtonLists, календари, Далее и т. д. Как и в текстовые поля, когда настройка интерфейса правки для включения других веб-элементов управления, используют следующие действия:

1. Добавление веб-элемента управления к `EditItemTemplate`.
2. Чтобы назначить соответствующие значения поля данных в соответствующее свойство, используйте синтаксис привязки данных.
3. В `UpdateCommand` контроля значения обработчик событий, программный доступ к Интернету и передайте его в соответствующий метод BLL.

В этом руководстве мы создадим более богатый интерфейс редактирования элемента управления DataList, содержащий элементов управления DropDownList и флажок. В частности, мы создадим элемент управления DataList, перечисляются сведения о продукте и разрешает имя продукта s, поставщика, категорию и снят с производства обновления (см. рис. 1).


[![Интерфейс редактирования содержит текстовое поле, двух элементов управления DropDownList и флажка](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Рис. 1**: интерфейс редактирования включает текстовое поле, двух элементов управления DropDownList и флажок ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Шаг 1: Отображение информации о продукте

Прежде чем создавать интерфейс редактирования s DataList, необходимо сначала создать интерфейс только для чтения. Сначала откройте `CustomizedUI.aspx` странице из `EditDeleteDataList` папку и, в режиме конструктора добавьте на страницу элемент управления DataList его `ID` свойства `Products`. S смарт-теге элемента управления DataList создайте новый ObjectDataSource. Назовите этот новый элемент управления ObjectDataSource `ProductsDataSource` и настройте его для получения данных из `ProductsBLL` класс s `GetProducts` метод. Как с предыдущих редактируемых элементов управления DataList руководствах мы обновим сведения s продукта непосредственно на уровне бизнес-логики. Соответственно установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет).


[![Задайте раскрывающиеся списки UPDATE, INSERT и DELETE вкладок (нет)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Рис. 2**: значение (None) обновления, вставки и удаления вкладки раскрывающиеся списки ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))


После настройки ObjectDataSource, Visual Studio автоматически создаст стандартный шаблон `ItemTemplate` для возвращенных элементов управления DataList, содержащий имя и значение для каждого из полей данных. Изменить `ItemTemplate` так, чтобы название продукта в списки шаблон `<h4>` элемент, а также имя категории, имя поставщика, цены и снят с производства. Кроме того, добавьте кнопку "Изменить", что его `CommandName` свойству редактирования. Декларативная разметка для моей `ItemTemplate` ниже:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Приведенная выше разметка размещает сведения о продукте с помощью &lt;h4&gt; заголовок для имени продукта s и четырех столбцов `<table>` в остальных полях. `ProductPropertyLabel` И `ProductPropertyValue` классы CSS, определенные в `Styles.css`, подробно обсуждаются в предыдущих учебных курсах. Рис. 3 показаны результаты выполненной работы в браузере.


[![Отображается имя, поставщика, категорию, более не поддерживается состояние и цену каждого продукта](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Рис. 3**: отображается имя поставщика, категорию, более не поддерживается состояние и цену каждого продукта ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Шаг 2: Добавление веб-элементов управления к интерфейсу редактирования

Первым этапом создания настраиваемых элементов управления DataList, интерфейс редактирования заключается в добавлении необходимых веб-элементы управления для `EditItemTemplate`. В частности мы должны элемента управления DropDownList для категории, другой — для поставщика и флажок для состояния снятия с производства. Так как цена продукта s не является доступным для редактирования в этом примере, мы можем его с помощью элемента управления Label Web отображения.

Чтобы настроить интерфейс редактирования, щелкните ссылку Изменить шаблоны в смарт-теге элемента управления DataList s и выберите `EditItemTemplate` параметр из раскрывающегося списка. Добавление элемента управления DropDownList для `EditItemTemplate` и задайте его `ID` для `Categories`.


[![Добавление элемента управления DropDownList для категорий](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Рис. 4**: Добавление элемента управления DropDownList для категорий ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))


Затем из смарт-тега DropDownList s, выберите параметр Выбор источника данных и создать новый ObjectDataSource, именуемый `CategoriesDataSource`. Настроить данный элемент управления ObjectDataSource для использования `CategoriesBLL` класс s `GetCategories()` метод (см. рис. 5). Затем DropDownList s мастер настройки источника данных предлагает для полей данных для каждого `ListItem` s `Text` и `Value` свойства. Иметь элемент управления DropDownList отображает `CategoryName` поля данных и использование `CategoryID` как значение, как показано на рис. 6.


[![Создайте новый ObjectDataSource, именуемый CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Рис. 5**: создайте новый ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))


[![Настройка отображения s DropDownList и значение поля](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Рис. 6**: Настройка DropDownList s отображения и значение поля ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))


Повторите шаги по созданию элемента управления DropDownList для списка suppliers в этой серии. Задайте `ID` для этого элемента управления DropDownList для `Suppliers` и назовите его ObjectDataSource `SuppliersDataSource`.

После добавления двух элементов управления DropDownList, добавьте флажок для состояния снятия с производства и текстовое поле для имени продукта s. Задайте `ID` для CheckBox и TextBox для `Discontinued` и `ProductName`, соответственно. Добавьте RequiredFieldValidator, чтобы убедиться, что пользователь предоставит значение для имени продукта s.

Наконец добавьте кнопки обновления и отмены. Помните, что для этих двух кнопок крайне важно, их `CommandName` свойств для обновления и отмены, соответственно.

Вы можете компоновать интерфейс правки, тем не менее вам нравится. Я решил использовать тот же четыре столбца ve `<table>` макета из интерфейса только для чтения, как следующий декларативный синтаксис и снимке экрана показано:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![Интерфейс правки вышел Laid как интерфейс только для чтения](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Рис. 7**: интерфейс редактирования — Laid Out, таких как интерфейс только для чтения ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Шаг 3: Создание EditCommand и обработчики событий CancelCommand

В настоящее время не существует синтаксис привязки данных в `EditItemTemplate` (за исключением `UnitPriceLabel`, которой была скопирована через дословно из `ItemTemplate`). Мы добавим синтаксис привязки данных сразу же, но первый let s создавать обработчики событий для элементов управления DataList s `EditCommand` и `CancelCommand` события. Помните, что ответственность за `EditCommand` обработчик событий — для отображения интерфейса редактирования для элемента управления DataList, была нажата, кнопка "Изменить", тогда как `CancelCommand` задание s предназначен для возвращения элемента управления DataList состояние до редактирования.

Создайте эти два обработчика событий и их с помощью следующего кода:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

С помощью этих двух обработчиков событий в месте, нажав кнопку "Изменить" отображает интерфейс редактирования и нажимая кнопку отмены возвращает элемент в режим только для чтения. Рис. 8 показан элемент управления DataList, после нажатия кнопки Edit для Chef Anton s Gumbo Mix. Так как мы ve еще, чтобы добавить любой синтаксис привязки данных к интерфейсу редактирования, `ProductName` текстовое поле пусто, `Discontinued` устанавливайте флажок, а первые элементы выбраны из `Categories` и `Suppliers` элементов управления DropDownList.


[![Щелкнув отображает кнопку редактирования интерфейса редактирования](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Рис. 8**: нажмите кнопку "Изменить" отображает интерфейс редактирования ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Шаг 4: Добавление синтаксис привязки данных к интерфейсу редактирования

Для отображения текущих значений s продукта интерфейс редактирования, нам нужно использовать синтаксис привязки данных для назначения значений полей данных на соответствующие значения веб-элемента управления. Синтаксис привязки данных могут применяться в конструкторе, перейдя на экран Редактирование шаблонов и выбрав ссылку Edit DataBindings из веб-элементы управления смарт-тегов. Кроме того синтаксис привязки данных можно добавить непосредственно в декларативной разметке.

Назначить `ProductName` значение поля данных `ProductName` TextBox s `Text` свойство, `CategoryID` и `SupplierID` значений для полей данных `Categories` и `Suppliers` элементов управления DropDownList `SelectedValue` свойства и `Discontinued` значение поля данных `Discontinued` флажок s `Checked` свойство. После внесения этих изменений, либо через конструктор, либо напрямую через декларативную разметку, вернемся к странице через браузер и нажмите кнопку кнопка Edit для Chef Anton s Gumbo Mix. Как показано на рис. 9, синтаксис привязки данных добавил текущие значения в текстовое поле, элементов управления DropDownList и флажок.


[![Щелкнув отображает кнопку редактирования интерфейса редактирования](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Рис. 9**: нажмите кнопку "Изменить" отображает интерфейс редактирования ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Шаг 5: Сохранение s изменения, внесенные пользователем в обработчике событий UpdateCommand

Когда пользователь изменяет продукт и щелкает кнопкой "Обновить", происходит обратная передача и DataList s `UpdateCommand` вызывает событие. В обработчике, нам нужно считывать значения из веб-элементов управления в `EditItemTemplate` и интерфейс со слоем BLL для обновления продукта в базе данных. Как мы видели в предыдущих руководствах ve `ProductID` обновленные продукта можно получить с помощью `DataKeys` коллекции. Введенный пользователем поля осуществляется с программного обращения к веб-элементов управления с помощью `FindControl("controlID")`, как показано в следующем коде:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

Код начинается по журналу `Page.IsValid` свойство, чтобы убедиться, что все элементы управления проверки на странице допустимы. Если `Page.IsValid` — `True`, продукта s `ProductID` значение считывается из `DataKeys` коллекции и ввода данных, веб-элементов управления в `EditItemTemplate` указываются программным способом. Далее считываются значения из этих веб-элементов управления в переменных, которые затем передаются в соответствующие `UpdateProduct` перегрузки. После обновления данных, DataList возвращается в состояние до редактирования.

> [!NOTE]
> Я ve опустить логика, добавленная в, обрабатывающего [обработка BLL и исключения уровня DAL](handling-bll-and-dal-level-exceptions-cs.md) учебника, чтобы сохранить код и в этом примере с фокусом ввода. В качестве упражнения эти функции можно добавьте после завершения этого учебника.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Шаг 6: Обработка NULL CategoryID и SupplierID значения

Позволяет базе данных "Борей" `NULL` значений в параметре `Products` таблицы s `CategoryID` и `SupplierID` столбцов. Тем не менее, в настоящее время размещения наших редактирования t интерфейс `NULL` значения. Если предпринимается попытка изменить продукт, `NULL` значение либо его `CategoryID` или `SupplierID` столбцы, мы получим `ArgumentOutOfRangeException` с сообщением об ошибке, аналогичную: *SelectedValue, который является недопустимым, так как она не имеет «Категории» не существует в списке элементов.* Кроме того, здесь s в настоящее время невозможно изменить поставщику или категории продуктов s значение отличным от`NULL` значение `NULL` один.

Для поддержки `NULL` значения для категории и поставщика элементов управления DropDownList, нам нужно добавить дополнительный `ListItem`. Я ve, выбранный для использования (нет) в качестве `Text` значение для данного `ListItem`, но если вы d, такие как можно изменить на что-то еще (например, пустая строка). Наконец, не забудьте установить элементов управления DropDownList `AppendDataBoundItems` для `True`; Если вы забудете сделать это, категории и привязке к DropDownList suppliers приведет к перезаписи статически добавляется `ListItem`.

После внесения этих изменений разметка элементов управления DropDownList в DataList s `EditItemTemplate` должен выглядеть следующим образом:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Статические `ListItem` s можно добавить через конструктор или напрямую через декларативный синтаксис элемента управления DropDownList. При добавлении элемента управления DropDownList для представления базы данных `NULL` значение, не забудьте добавить `ListItem` через декларативный синтаксис. Если вы используете `ListItem` редактор коллекции в конструкторе, созданный декларативный синтаксис не будет включать `Value` параметр полностью, если назначить содержит пустую строку, создание декларативная разметка, таких как: `<asp:ListItem>(None)</asp:ListItem>`. Хотя это может показаться безопасным, отсутствующий `Value` приводит к DropDownList для использования `Text` значение свойства на его месте. Означает, что если это `NULL` `ListItem` — флажок установлен, значение (None) будет предпринята попытка присвоить поля данных продукта (`CategoryID` или `SupplierID`, в этом руководстве), что приведет к возникновению исключения. Явно задать `Value=""`, `NULL` продукта будет присвоено значение поля данных, когда `NULL` `ListItem` выбран.


Отвлекитесь и просмотрите ход работы через браузер. При правке продукта, обратите внимание, что `Categories` и `Suppliers` у обоих элементов управления DropDownList (нет) параметр в начале элемента управления DropDownList.


[![Категории и элементов управления DropDownList Suppliers включают (None) параметр](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Рис. 10**: `Categories` и `Suppliers` элементов управления DropDownList включают (None) параметра ([Просмотр полноразмерного изображения](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))


Чтобы сохранить (нет) параметр как база данных `NULL` значение, необходимо вернуться к `UpdateCommand` обработчик событий. Изменение `categoryIDValue` и `supplierIDValue` переменные, допускающие значения NULL целых чисел и назначить их значение отличное от `Nothing` только если DropDownList s `SelectedValue` не является пустой строкой:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Благодаря этому изменению значение `Nothing` будут переданы `UpdateProduct` параметр метода BLL, если пользователь выбрал (нет) из любой из раскрывающихся списков, которые соответствует `NULL` базы данных значение.

## <a name="summary"></a>Сводка

В этом руководстве мы увидели, как создать более сложные интерфейс редактирования элементов управления DataList, включены три разных входных веб-элементы управления TextBox, двух элементов управления DropDownList и флажок, а также элементов управления проверки. При создании интерфейса правки, действия одинаковы независимо от того, веб-элементов управления используется: Начните с добавления веб-элементов управления DataList s `EditItemTemplate`; присвоить соответствующие значения поля данных соответствующие веб-использовать синтаксис привязки данных свойства элемента управления; и в `UpdateCommand` обработчик событий программного доступа к веб-элементов управления и их соответствующие свойства, передайте их значения в BLL.

При создании интерфейс редактирования ли он s состоит просто текстовые поля или коллекции разных веб-элементов управления, убедитесь, что для правильной обработки базы данных `NULL` значения. При учете `NULL` s, крайне необходимо, вы не только правильного отображения существующего `NULL` значение в существующем интерфейсе правки, но также, которые предоставляют средства для классификации и пометки значения как `NULL`. Для элементов управления DropDownList в DataLists, это обычно означает Добавление статического `ListItem` которого `Value` явным образом присваивается пустая строка (`Value=""`) и небольшой фрагмент кода для добавления `UpdateCommand` обработчик событий для определения того, является ли `NULL``ListItem` был выбран.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Dennis Patterson, Дэвид Suru и Рэнди Шмидт, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [Вперед](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
