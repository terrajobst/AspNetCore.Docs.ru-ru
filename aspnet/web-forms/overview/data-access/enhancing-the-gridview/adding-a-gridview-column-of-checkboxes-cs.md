---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Добавление столбца GridView флажков (C#) | Документы Microsoft
author: rick-anderson
description: В этом учебнике рассматривается, как добавить столбец флажков для элемента управления GridView для выбора нескольких строк G. очевидно, как предоставить пользователю...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c9ad4c66c5bdd24df691180acf354f666e6e578
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880171"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Добавление столбца GridView флажков (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) или [скачать PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> Этот учебник рассказывается, как добавить столбец флажков для элемента управления GridView предоставить пользователю с интуитивно понятным способом выбора нескольких строк элемента управления GridView.


## <a name="introduction"></a>Вступление

В предыдущем учебнике мы рассмотрели, как добавить столбец кнопок-переключателей в GridView с целью выбора конкретной записи. Столбец кнопок-переключателей — это подходящий пользовательский интерфейс, когда пользователь имеет ограничение в выбор не более одного элемента в сетке. В некоторых случаях Однако мы может потребоваться разрешить пользователю выбрать произвольное число элементов в сетке. Клиенты веб сервера электронной почты, например, обычно отображают список сообщений со столбцом флажки. Пользователь может выбрать произвольное количество сообщений и затем выполнить некоторые действия, например перемещать сообщения электронной почты в другую папку или их удаления.

В этом учебнике вы узнаете, как добавить столбец флажков и как определить, какие флажки были возвращены при обратной передаче. В частности мы выполним сборку пример, в котором точно имитирует пользовательский интерфейс клиента веб сервера электронной почты. Наш пример будет включать выгружаемого GridView с перечнем продуктов в `Products` таблицы базы данных с флажком в каждой строке (см. рис. 1). Удаление выбранных продуктов при нажатии кнопки, приведет к удалению этих продуктов выбран.


[![Каждая строка продукта включает флажок](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Рис. 1**: каждая строка продукта включает Checkbox ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Шаг 1: Добавить выгружаемого GridView, содержащий сведения о продукте

Прежде чем мы беспокоиться о добавлении столбца флажков, позволяют s сначала изучить перечнем продуктов в GridView, поддерживающий разбиение на страницы. Сначала откройте `CheckBoxField.aspx` страницы в `EnhancedGridView` папки и перетащите элемент управления GridView с панели элементов в конструктор параметр его `ID` для `Products`. Затем выберите привязка GridView к новой ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource для использования `ProductsBLL` класса вызов `GetProducts()` метод для возвращения данных. Поскольку это GridView будет только для чтения, установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет).


[![Создать новый элемент управления ObjectDataSource ProductsDataSource с именем](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**На рисунке 2**: создать новый именованный ObjectDataSource `ProductsDataSource` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Настройка ObjectDataSource для извлечения данных с помощью метода GetProducts()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Рис. 3**: Настройка ObjectDataSource для извлечения данных с помощью `GetProducts()` метод ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Рис. 4**: задать раскрывающихся списков в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


После завершения работы мастера настройки источника данных Visual Studio автоматически создаст BoundColumns и CheckBoxColumn для полей данных, связанных с продуктом. Как было сделано в предыдущем учебнике, удалите все, кроме `ProductName`, `CategoryName`, и `UnitPrice` стояли и измените `HeaderText` свойства с продуктом, категории и цены. Настройка `UnitPrice` BoundField, чтобы его значение форматируются как денежное значение. Кроме того, настройте GridView для поддержки разбиения на страницы, установив флажок включения постраничного просмотра из смарт-тега.

Разрешить s также добавить пользовательский интерфейс для удаления выбранных продуктов. Добавить кнопку веб-элемент управления под GridView, установка его `ID` для `DeleteSelectedProducts` и его `Text` свойство для удаления выбранных продуктов. Вместо фактического удаления продуктов из базы данных, в этом примере мы будем просто отображают сообщение о том, продукты, которые будут удалены. Чтобы решить эту проблему, добавьте веб-управления Label под кнопкой. Назначить ИД, `DeleteResults`снимите out его `Text` и установите его `Visible` и `EnableViewState` свойства `false`.

После внесения этих изменений, GridView, ObjectDataSource, кнопку и метку s декларативная разметка должен быть аналогичен следующему:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Занять некоторое время, чтобы перейти на страницу в браузере (см. рис. 5). На этом этапе вы увидите имя, категорию и цены продуктов первых десяти.


[![Имя, категорию и цены продуктов первый десять перечислены](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Рис. 5**: Name, категории и цены продуктов первый десять перечислены ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Шаг 2: Добавление столбца флажков

Поскольку ASP.NET 2.0 включает CheckBoxField, может показаться, что он может использоваться для добавления столбца с флажками в GridView. К сожалению это не так, как CheckBoxField предназначен для работы с полем данных Boolean. То есть для использования CheckBoxField нам необходимо указать базовое поле данных, значение которого был запрошен, чтобы определить, установлен ли флажок, готовый для просмотра. Не удается использовать CheckBoxField только включать столбец unchecked флажки.

Вместо этого необходимо добавить TemplateField и добавить элемент управления CheckBox Web его `ItemTemplate`. Добавим TemplateField для `Products` GridView и сделать его первого поля (дальней слева). В смарт-теге s GridView, щелкните ссылку Изменить шаблоны, а затем перетащите флажок веб-элемент управления из области элементов в `ItemTemplate`. Установите этот флажок s `ID` свойства `ProductSelector`.


[![Добавление элемента управления CheckBox Web с именем ProductSelector на ItemTemplate s TemplateField](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Рис. 6**: добавить флажок веб-элемента управления имя `ProductSelector` в TemplateField s `ItemTemplate` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


TemplateField и веб-флажок управления добавлен каждая строка теперь включает флажок. На рисунке 7 показано эту страницу при просмотре через браузер, после добавления TemplateField и флажки.


[![Каждая строка продукта теперь включает флажок](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Рис. 7**: каждая строка продукта теперь включает Checkbox ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Шаг 3: Определение какие флажки были возвращены при обратной передаче

На этом этапе у нас есть столбец флажков, но не может определить, какие флажки были возвращены при обратной передаче. При нажатии кнопки Удалить выбранные продукты, нам нужно знать, какие флажки были возвращены для удаления этих продуктов.

GridView s [ `Rows` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) предоставляет доступ к строкам данных в GridView. Мы итерацию этих строк программного доступа к элемента управления CheckBox, а затем обратитесь к его `Checked` свойства, чтобы определить, был ли выбран флажок.

Создайте обработчик событий для `DeleteSelectedProducts` веб-управления Button s `Click` событий и добавьте следующий код:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

`Rows` Свойство возвращает коллекцию `GridViewRow` экземпляров этого состава строки данных s GridView. `foreach` Здесь цикле перечисляет этой коллекции. Для каждого `GridViewRow` объекта строки s флажок программно осуществляется с помощью `row.FindControl("controlID")`. Если этот флажок установлен, соответствующие строки s `ProductID` значение извлекается из `DataKeys` коллекции. В этом упражнении мы просто отобразить информационное сообщение в `DeleteResults` меткой, несмотря на то, что в работающее приложение d мы вместо вызова с `ProductsBLL` класса s `DeleteProduct(productID)` метод.

С появлением этого обработчика событий, при нажатии кнопки Удалить выбранные продукты теперь отображается `ProductID` s выбранных продуктов.


[![При нажатии кнопки Удалить выбранные продукты перечислены ProductIDs выбранных продуктов](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Рис. 8**: при удалить выбранные продукты кнопки выбранные продукты `ProductID` перечислены ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Шаг 4: Добавление отметьте все и снимите флажки всех кнопок

Если пользователю необходимо удалить все продукты на текущей странице, их необходимо проверить каждый из десяти флажки. Мы можем помочь ускорить этот процесс, добавив проверьте все кнопка, при нажатии выбирает все флажки в сетке. Снять все кнопки желательно иметь одинаково.

Добавьте две кнопки веб-элементов управления на страницу, поместив их над элементом управления GridView. Задать первый из них s `ID` для `CheckAll` и его `Text` свойство для проверки всех; набор второй один s `ID` для `UncheckAll` и его `Text` снять все свойства.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Создайте метод с именем класса кода программной `ToggleCheckState(checkState)` , при вызове перечисляет `Products` GridView s `Rows` коллекции и каждого s флажок задает `Checked` значение переданного свойства в *checkState*  параметра.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Создайте `Click` обработчики событий для `CheckAll` и `UncheckAll` кнопки. В `CheckAll` обработчик событий s, просто вызов `ToggleCheckState(true)`, в `UncheckAll`, вызовите `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Этот код нажав кнопку Проверить все вызывает обратную передачу и проверяет все флажки в GridView. Аналогичным образом щелкнув снять все приводит к отмене выбора все флажки. Рис. 9 показана экрана после проверки проверьте все кнопки.


[![Нажав кнопку все платежный выбирает все флажки](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Рис. 9**: щелкнув проверьте все кнопки выбирает все флажки ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Если отображение столбца флажков, один из способов установив или сняв все флажки, через флажок в строке заголовка. Кроме того текущий отметьте все / снять все реализации требует обратной передачи. Флажки может быть установлен или снят, тем не менее, полностью основана на клиентский сценарий, тем самым обеспечивая точную взаимодействие с пользователем. Чтобы исследовать с помощью флажка строку заголовка для проверки всех и снимите все подробно вместе с методами клиентские обсуждение извлечь [проверка все флажки в GridView с использованием клиентского скрипта и проверьте все флажок](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Сводка

В случаях, когда нужно позволить пользователям выбирать произвольное число строк в GridView, прежде чем продолжить добавление столбца флажки — один из вариантов. Как было показано в этом учебнике, включая столбец флажки в GridView влечет за собой Добавление TemplateField с флажок веб-элемент управления. С помощью веб-элемента управления (а не вводится разметки непосредственно в шаблон, как это делалось в предыдущем учебнике) ASP.NET автоматически запоминается, что флажки были и не были проверены на обратную передачу. Можно также программным образом обратиться к флажки в коде для определения, является ли данный флажок установлен, или для изменения состояния флажка.

Этот учебник и последним рассматривали Добавление селектор строк столбца GridView. В следующем учебном курсе мы изучим, как это сделать, с немного усилий, мы можем добавить возможности вставки GridView.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Вперед](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
