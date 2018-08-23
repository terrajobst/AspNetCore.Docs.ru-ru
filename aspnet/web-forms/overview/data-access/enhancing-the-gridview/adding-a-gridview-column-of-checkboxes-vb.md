---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Добавление столбца GridView флажков (VB) | Документация Майкрософт
author: rick-anderson
description: Этот учебник рассказывает, как добавление столбца флажков элемента управления GridView для предоставления пользователю интуитивно понятным способом выбора нескольких строк G....
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: be7fc4fe93d15e7550f873adffa15e82384d25be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829398"
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Добавление столбца GridView флажков (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) или [скачать PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> Этом руководстве рассказывается, как добавление столбца флажков элемента управления GridView для предоставления пользователю выбора нескольких строк GridView интуитивно понятным способом.


## <a name="introduction"></a>Вступление

В предыдущем учебном курсе было рассмотрено, как добавить столбец кнопок-переключателей в GridView для выбора определенной записи. Столбец кнопок-переключателей — это подходящий пользовательский интерфейс, когда пользователь ограничена выбором не более одного элемента из сетки. В некоторых случаях тем не менее, мы может потребоваться разрешить пользователю выбрать произвольное число элементов в сетке. Например, веб-почтовые клиенты, обычно отображают список сообщений с помощью столбца флажков. Пользователь может выбрать произвольное число сообщений и затем выполняют некоторые действия, такие как перемещение сообщений электронной почты в другую папку или удаление.

В этом руководстве вы узнаете, как добавить столбец флажков и как определить, какие флажки были возвращены при обратной передаче. В частности мы создадим пример, в котором точно имитирует пользовательский интерфейс клиента веб-службы электронной почты. Наш пример будет включать разбитого на страницы элемента управления GridView выводе продуктов в `Products` таблицы базы данных с флажком в каждой строке (см. рис. 1). Удаление выбранных продуктов при нажатии кнопки, приведет к удалению этих продуктов выбран.


[![Каждая строка продукта содержит флажок](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Рис. 1**: каждая строка продукта содержит флажки ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Шаг 1: Добавление элемента GridView разбитого на страницы, сведения о продукте

Прежде чем думать о добавлении столбца флажков, позвольте s сначала остановиться на перечисление продуктов в элементе управления GridView, который поддерживает разбиение по страницам. Сначала откройте `CheckBoxField.aspx` странице в `EnhancedGridView` папки и перетащите элемент управления GridView с панели элементов в конструктор, установив его `ID` для `Products`. Затем выберите для привязки GridView к элементу управления ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource на использование `ProductsBLL` , вызова `GetProducts()` метод для возврата данных. Так как этот GridView будет доступен только для чтения, установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет).


[![Создайте новый ObjectDataSource, именуемый ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Рис. 2**: создайте новый ObjectDataSource с именем `ProductsDataSource` ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Настройте элемент ObjectDataSource для извлечения данных с помощью метода GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Рис. 3**: Настройка ObjectDataSource для извлечения данных с помощью `GetProducts()` метод ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Рис. 4**: задать раскрывающиеся списки в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


После завершения работы мастера настройки источников данных Visual Studio автоматически создаст BoundColumns и CheckBoxColumn для полей данных, связанных с продуктом. Как это делалось в предыдущем руководстве, удалите все, кроме `ProductName`, `CategoryName`, и `UnitPrice` полей BoundField и измените `HeaderText` свойства категорию и цену продукта. Настройка `UnitPrice` BoundField таким образом, чтобы его значение форматируется как денежная единица. Кроме того, настройте поддержку разбиения по страницам, установив флажок Включить разбиение по страницам в смарт-теге GridView.

Позвольте s также добавить пользовательский интерфейс для удаления выбранных продуктов. Добавить кнопку веб-элемент управления под GridView, установка его `ID` для `DeleteSelectedProducts` и его `Text` свойство, чтобы удалить выбранные продукты. Вместо фактического удаления продуктов из базы данных, в этом примере мы просто отобразим сообщение о том, продукты, которые будут удалены. Чтобы решить эту проблему, добавьте элемент управления Label Web, под кнопкой. Назначить ИД, `DeleteResults`снимите out его `Text` и установите его `Visible` и `EnableViewState` свойства `False`.

После внесения этих изменений, GridView, элемент управления ObjectDataSource, кнопку и метку s декларативная разметка должен быть аналогичен следующему:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Отвлекитесь и просмотреть страницу в браузере (см. рис. 5). На этом этапе вы увидите имя, категорию и цену первых десяти продуктов.


[![Имя, категорию и цену первых десяти продуктов](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Рис. 5**: указаны имя, категорию и цену первых десяти продуктов ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Шаг 2: Добавление столбца флажков

Поскольку ASP.NET 2.0 включает по полю CheckBoxField, может показаться, что он может использоваться для добавления столбца флажков к GridView. К сожалению это не так, как CheckBoxField предназначен для работы с полем данных Boolean. То есть для использования CheckBoxField мы необходимо указать базовое поле данных, значение которого был запрошен, чтобы определить, установлен ли флажок, готовый для просмотра. Мы нельзя использовать CheckBoxField для только что включать в себя столбец unchecked флажков.

Вместо этого необходимо добавить поле TemplateField и добавить флажок веб-элемент управления для его `ItemTemplate`. Добавим TemplateField для `Products` GridView и добавьте его в первое поле (крайней левой). Смарт-теге GridView s, щелкните ссылку Изменить шаблоны, а затем перетащите флажок веб-элемент управления из области элементов в `ItemTemplate`. Установите этот флажок s `ID` свойства `ProductSelector`.


[![Добавить флажок веб-элемент управления с именем ProductSelector элементу TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Рис. 6**: добавить именованный элемент управления CheckBox Web `ProductSelector` в TemplateField s `ItemTemplate` ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


TemplateField и веб-флажок управления добавлен каждая строка теперь включает флажок. Рис. 7 Эта страница отображается, при просмотре через браузер, после добавления TemplateField и CheckBox.


[![Каждая строка продукта теперь включает флажок](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Рис. 7**: каждая строка продукта теперь включает флажок ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Шаг 3: Определение, какие флажки были возвращены при обратной передаче

На этом этапе у нас есть столбец с флажками, но не может определить, какие флажки были возвращены при обратной передаче. При нажатии кнопки удаления выбранных продуктов, однако необходимо знать, какие флажки были проверены для удаления этих продуктов.

GridView s [ `Rows` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) предоставляет доступ к строкам данных в GridView. Мы может выполнять итерацию эти строки программного доступа к элемента управления CheckBox и затем обратитесь к его `Checked` свойства, чтобы определить, была ли выбрана флажок.

Создайте обработчик событий для `DeleteSelectedProducts` веб-управления Button s `Click` событий и добавьте следующий код:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows` Свойство возвращает коллекцию `GridViewRow` экземпляров, состав строки данных s GridView. `For Each` Цикл здесь перечисляет этой коллекции. Для каждого `GridViewRow` объект, строка s флажок программным образом осуществляется с помощью `row.FindControl("controlID")`. Если этот флажок установлен, соответствующий строке s `ProductID` значение извлекается из `DataKeys` коллекции. В этом упражнении мы просто вывода информационного сообщения в `DeleteResults` метки, несмотря на то, что в работающее приложение d мы вместо этого отправьте вызов в `ProductsBLL` класс s `DeleteProduct(productID)` метод.

Теперь нажмите кнопку Удалить выбранных продуктов отображаются с добавлением этого обработчика событий, `ProductID` s выбранных продуктов.


[![При нажатии кнопки Удалить выбранные продукты перечислены ProductIDs продукты выбранного](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Рис. 8**: при удалить выбранные продукты кнопки продукты выбранного `ProductID` перечислены ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Шаг 4: Добавление отметьте все и снимите флажки всех кнопок

Если пользователь хочет удалить все продукты на текущей странице, их необходимо проверить каждый из десяти флажки. Мы можем помочь ускорить этот процесс, добавив проверьте все кнопка, при щелчке, выбирает все флажки в сетке. Снять все кнопка будет одинаково.

Добавьте две кнопки веб-элементов управления на страницу, поместив их над элементом управления GridView. Задать первый диск s `ID` для `CheckAll` и его `Text` проверять все свойства; набор второго s `ID` для `UncheckAll` и его `Text` снять все свойства.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Создайте метод в класс фонового кода, с именем `ToggleCheckState(checkState)` , при вызове перечисляет `Products` GridView s `Rows` коллекцию и задает каждого s флажок `Checked` значение переданного свойства в *свойство checkState*  параметра.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Создайте `Click` обработчики событий для `CheckAll` и `UncheckAll` кнопки. В `CheckAll` обработчик событий s, простым вызовом `ToggleCheckState(True)`, в списке `UncheckAll`, вызовите `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Этот код нажав кнопку Проверить все вызывает обратную передачу и проверяет все флажки в GridView. Аналогичным образом щелкнув снять все приводит к отмене выбора всех флажков. Рис. 9 показан экран после проверки проверьте все кнопки.


[![Щелкнув кнопку все платежный выбирает все флажки](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Рис. 9**: щелкнув проверьте все кнопки выбирает все флажки ([Просмотр полноразмерного изображения](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Если отображение столбца флажков, установив или сняв все флажки один из способов является через флажок в строке заголовка. Кроме того текущий отметьте все / снять все реализации требуется обратную передачу. Флажки может быть установлен или снят, тем не менее, полностью основана на скрипт на стороне клиента, тем самым обеспечивая точную и аккуратную работу. Для просмотра с помощью флажка строку заголовка для проверки всех и снимите все подробно, а также обсуждение с помощью методов со стороны клиента, ознакомьтесь с [проверки все флажки в GridView с помощью клиентского скрипта и проверьте все флажок](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Сводка

В случаях, когда необходимо позволить пользователям выбирать произвольное число строк в элементе управления GridView, прежде чем продолжить добавление столбца флажков является один из вариантов. Как мы видели в этом руководстве, включая столбец флажков в GridView влечет добавление TemplateField с флажок веб-элемент управления. С помощью веб-элемент управления (а не внедряет разметку непосредственно в шаблоне, как это делалось в предыдущем учебнике) ASP.NET автоматически запоминает, какие флажки были и не были возвращены на обратную передачу. Можно также программно обратиться к флажки в коде для определения того, установлен ли флажок данного флажка или чтобы изменить состояние проверки.

В этом учебнике и последним рассмотрели Добавление столбца селектор строк к GridView. В нашем следующем учебном курсе мы рассмотрим, как это сделать, с помощью небольшой работы, можно добавить возможности вставки к GridView.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-a-gridview-column-of-radio-buttons-vb.md)
> [Вперед](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
