---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: С помощью TemplateFields в элементе управления DetailsView (C#) | Документы Microsoft
author: rick-anderson
description: Же TemplateFields возможности, доступные в GridView, также доступны с помощью элемента управления DetailsView. В этом учебнике мы будем отображения одного продукта...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f1d2e8312451c0bd1b3aba448963317f5fe06029
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879115"
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>С помощью TemplateFields в элементе управления DetailsView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) или [скачать PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Же TemplateFields возможности, доступные в GridView, также доступны с помощью элемента управления DetailsView. В этом учебнике мы отображения одного продукта во время использования DetailsView, содержащий TemplateFields.


## <a name="introduction"></a>Вступление

TemplateField предлагает более высокую степень гибкие возможности визуализации данных, чем BoundField, CheckBoxField, HyperLinkField и других элементов управления полем данных. В [с предыдущим учебником](using-templatefields-in-the-gridview-control-cs.md) мы рассмотрели использование TemplateField в GridView, чтобы:

- Отображение нескольких значений полей данных в одном столбце. В частности, оба `FirstName` и `LastName` поля были объединены в один столбец GridView.
- Используйте альтернативный веб-элемент управления для выражения значения поля данных. Мы узнали, как Показать `HiredDate` значение с помощью элемента управления Calendar.
- Показать сведения о состоянии на основе базовых данных. Хотя `Employees` таблица не содержит столбец, который возвращает количество дней, сотрудник был на работе, нам удалось содержат такие данные в примере GridView в предыдущем учебнике с помощью TemplateField и метод форматирования.

Же TemplateFields возможности, доступные в GridView, также доступны с помощью элемента управления DetailsView. В этом учебнике мы отображения одного продукта во время использования DetailsView, который содержит два TemplateFields. Первый TemplateField объединит `UnitPrice`, `UnitsInStock`, и `UnitsOnOrder` полей данных в одну строку DetailsView. Второй TemplateField отобразит значение `Discontinued` , но будет использовать метод форматирования для отображения «YES», если `Discontinued` — `true`и «NO» в противном случае.


[![Два TemplateFields используются для настройки отображения](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Рис. 1**: два TemplateFields используются для настройки отображения ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


Давайте начнем!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Шаг 1: Привязка данных в DetailsView

Как описано в предыдущем учебнике при работе с TemplateFields, часто бывает проще всего начать с создания управления DetailsView, который содержит только стояли, а затем добавьте новый TemplateFields или преобразовать существующий стояли TemplateFields как требуется. . Таким образом, это руководство по началу работы добавление DetailsView на страницу в конструкторе и привяжите его к элементу ObjectDataSource, который возвращает список продуктов. Эти шаги создаст DetailsView с стояли для каждого продукта Нелогические значения полей и CheckBoxField одно поле Логическое значение (неподдерживаемые).

Откройте `DetailsViewTemplateField.aspx` страницы и перетащите DetailsView из области элементов в конструктор. В смарт-тег DetailsView команду, чтобы добавить новый элемент управления ObjectDataSource, который вызывает `ProductsBLL` класса `GetProducts()` метод.


[![Добавить новый элемент управления ObjectDataSource, который вызывает метод GetProducts()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**На рисунке 2**: Добавление нового элемента управления ObjectDataSource, методы Invoke `GetProducts()` метод ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


В этом отчете удалить `ProductID`, `SupplierID`, `CategoryID`, и `ReorderLevel` стояли. После этого изменения порядка стояли, чтобы `CategoryName` и `SupplierName` стояли отображаются сразу после `ProductName` BoundField. Вы можете настроить `HeaderText` свойства и свойства форматирования стояли как усмотрению. Как и с GridView, эти изменения уровня BoundField могут выполняться через диалоговое окно поля (доступными, щелкнув ссылку Изменить поля DetailsView смарт-тега) или с помощью декларативного синтаксиса. Наконец, очистить DetailsView `Height` и `Width` значения свойств, чтобы разрешить DetailsView управления для развертывания на основе отображаемых данных и установите флажок Включить разбиение по страницам в смарт-тег.

После внесения этих изменений, управления DetailsView должна выглядеть следующим образом:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Занять некоторое время для просмотра страницы через браузер. На этом этапе вы увидите одного продукта в списке (Chai) со строками, показывающая название продукта, категории, поставщика, цены, единиц на складе, единицы в заказе и его состояние больше не поддерживаются.


[![Сведения о продукте, отображаются с помощью ряда стояли](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Рис. 3**: сведения о продукте, отображаются с помощью ряда стояли ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Шаг 2: Объединение цену, единиц на складе и модули в порядке в одной строке

DetailsView имеет одну строку для `UnitPrice`, `UnitsInStock`, и `UnitsOnOrder` поля. Можно объединить эти поля данных в одну строку с TemplateField, добавив новый TemplateField или путем преобразования одну из существующих `UnitPrice`, `UnitsInStock`, и `UnitsOnOrder` стояли в TemplateField. Хотя я предпочитаю лично, преобразование существующих стояли, давайте практики, добавив новый TemplateField.

Запустить, щелкнув ссылку Изменить поля в DetailsView смарт-тег, чтобы открыть диалоговое окно «поля». Далее добавьте новый TemplateField и задать его `HeaderText` свойство «Цена и инвентаризации» и перемещения новый TemplateField, так чтобы оно расположено над `UnitPrice` BoundField.


[![Добавьте новый TemplateField управления DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Рис. 4**: Добавление новых TemplateField управления DetailsView ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


Поскольку этот новый TemplateField будет содержать значения, отображаемого в текущий момент `UnitPrice`, `UnitsInStock`, и `UnitsOnOrder` стояли, удалим их.

Последняя задача для выполнения этого шага — для определения `ItemTemplate` разметку для цены и TemplateField инвентаризации, который можно сделать либо с помощью редактирования интерфейса в конструкторе или вручную посредством декларативного синтаксиса элемента управления DetailsView шаблона. Как и в GridView, интерфейс редактирования шаблона DetailsView возможен, щелкнув ссылку "Правка шаблонов" смарт-тега. Здесь можно выбрать шаблон для редактирования из раскрывающегося списка и затем добавьте любой веб-элементы управления из области элементов.

В этом учебнике, начните с добавления элемента управления Label цены и TemplateField инвентаризации `ItemTemplate`. Затем щелкните ссылку изменить привязки данных в смарт-теге метка веб-элемента управления и привязать `Text` свойства `UnitPrice` поля.


[![Привязать свойство текст метки в поле «Цена» данных](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Рис. 5**: привязка метки `Text` свойства `UnitPrice` поля данных ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Форматирование цены как денежная единица

В результате этого добавления веб-управления Label цены и TemplateField инвентаризации будет отображаться только цену для выбранного продукта. Рис. 6 показан снимок экрана ход работы сих при просмотре через браузер.


[![Цены и TemplateField инвентаризации показывает цену](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Рис. 6**: цены и TemplateField инвентаризации показывает цену ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


Обратите внимание, что цена продукта не отформатирован в виде валюты. С BoundField форматирование возможна, задав `HtmlEncode` свойства `false` и `DataFormatString` свойства `{0:formatSpecifier}`. Для TemplateField тем не менее, любой инструкции форматирования необходимо указать в синтаксисе привязки данных или с помощью метода форматирования, определенные где-либо в коде приложения (например, в класс кода страницы ASP.NET).

Чтобы задать формат для привязки данных синтаксис, используемый в веб-управления Label, вернитесь диалоговое окно Привязка данных, щелкните ссылку изменить привязки к данным из метки смарт-тега. Можно ввести непосредственно в раскрывающемся списке Формат инструкции форматирования или выберите один из определенных строк форматирования. Как и с BoundField `DataFormatString` свойства, форматирование задается с помощью `{0:formatSpecifier}`.

Для `UnitPrice` используйте форматирование валюты указан путем выбора значения соответствующего раскрывающегося списка или введя в поле `{0:C}` вручную.


[![Цена форматируется как денежная единица](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Рис. 7**: форматирование цены как валюты ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


Декларативно, спецификации форматирования указывается в качестве второго параметра в `Bind` или `Eval` методы. Настройки, выполненные через конструктор результат в следующее выражение привязки данных в декларативная разметка:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Добавление оставшихся полей данных в TemplateField

На этом этапе отображения и форматирования `UnitPrice` данные поле в TemplateField инвентаризации и цену, но по-прежнему нужно отобразить `UnitsInStock` и `UnitsOnOrder` поля. Позволяет отображать их на строку ниже цены и в круглые скобки. Интерфейс редактирования шаблона, в конструкторе можно добавить такие разметки, поместив курсор в шаблоне и просто введя текст, который отображается. Кроме того эта разметка можно ввести непосредственно в декларативного синтаксиса.

Добавление разметки static, метка веб-элементов управления и синтаксис привязки данных, чтобы цена и TemplateField инвентаризации сведения о цене и инвентаризации следующим образом:

*«Цена»*  
(**На складе / заказа:** *UnitsInStock* / *UnitsOnOrder*)

После выполнения этой задачи в DetailsView должна выглядеть следующим образом:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Эти изменения мы цену и инвентаризации сведения собраны в одну строку DetailsView.


[![Цены и данные инвентаризации отображается в одной строки](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Рис. 8**: одной строки отображаются цены и данные инвентаризации ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Шаг 3: Настройка сведений неподдерживаемые поля

`Products` Таблицы `Discontinued` столбец имеет значение бита, указывает ли продукт больше не поддерживается. При привязке DetailsView (или GridView) к элементу управления источником данных, таких как поля логическое значение, `Discontinued`, реализуются в виде CheckBoxFields, тогда как Нелогические значения полей, таких как `ProductID`, `ProductName`, и т. д., реализованы в виде Стояли. CheckBoxField готовится к просмотру как отключенный элемент управления checkbox, проверяется, если значение поля данных в противном случае — значение True и этот флажок снят.

Вместо отображения CheckBoxField, необходимо вместо этого отобразить текстом, указывающим ли продукт не поддерживается. Для выполнения этой задачи может удалить CheckBoxField из DetailsView и затем добавьте поле BoundField которого `DataField` было задано значение `Discontinued`. Теперь пора это сделать. После этого изменения DetailsView отображается текст «True» для неподдерживаемые продукты и «False» для продуктов, которые все еще активны.


[![Эти строки True и False используются для отображения состояния больше не поддерживаются](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Рис. 9**: строки True и False, используются для отображения состояния Discontinued ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


Предположим, что мы не хотим видеть строки «True» или «False» для использования, но «YES» и «NO», вместо этого. Такие настройки могут выполняться с помощью TemplateField и метод форматирования. Метод форматирования можно выполнять в любое количество входных параметров, но должны возвращать HTML (в виде строки) для вставки в шаблоне.

Добавьте метод форматирования `DetailsViewTemplateField.aspx` на странице кода класса с именем `DisplayDiscontinuedAsYESorNO` , принимает значение типа Boolean в качестве входного параметра и возвращает строку. Как описано в предыдущем учебнике, этот метод *должен* помечается как `protected` или `public` чтобы быть доступен из шаблона.


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Этот метод проверяет входной параметр (`discontinued`) и «Да» в противном случае возвращается `true`, в противном случае — «NO».

> [!NOTE]
> В методе форматирования проверяются в предыдущем учебника Напомним, что мы передавался в поле данных, которое может содержать `NULL` s и поэтому требуется проверка при сотрудника `HiredDate` значение свойства была база данных `NULL` value, прежде чем доступ к `EmployeesRow`в `HiredDate` свойство. Эту проверку не требуется здесь с момента `Discontinued` столбец никогда не может иметь базу данных `NULL` присвоенных значений. Кроме того, именно поэтому метод принимает логическое значение входного параметра, вместо того чтобы принять `ProductsRow` экземпляра или тип параметра `object`.


С помощью этого форматирования завершения метода остается будет вызывать его из TemplateField `ItemTemplate`. Чтобы создать TemplateField либо удалите `Discontinued` BoundField и добавить новый TemplateField, или преобразовать `Discontinued` BoundField в TemplateField. Затем с помощью представления декларативная разметка изменить TemplateField, чтобы он содержал только ItemTemplate, который вызывает `DisplayDiscontinuedAsYESorNO` метод, передавая значение текущей `ProductRow` экземпляра `Discontinued` свойство. Эта возможность доступна через `Eval` метод. В частности TemplateField разметки должен выглядеть так:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Это приведет к `DisplayDiscontinuedAsYESorNO` метода, вызываемого при подготовке к просмотру DetailsView, передав `ProductRow` экземпляра `Discontinued` значение. Поскольку `Eval` метод возвращает значение типа `object`, но `DisplayDiscontinuedAsYESorNO` метода требуется входной параметр типа `bool`, мы приведение `Eval` методы возвращают значение `bool`. `DisplayDiscontinuedAsYESorNO` Метод вернет «Да» или «NO» в зависимости от значения получит. Возвращаемое значение отображается в этом DetailsView строк (см. рис. 10).


[![Да или нет значения: В строке Discontinued](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Рис. 10**: Да или нет значения: В строке Discontinued ([Просмотр полноразмерное изображение](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>Сводка

TemplateField в элементе управления DetailsView обеспечивает более высокую степень гибкости в отображении данных не входит в состав других элементов управления полем и идеально подходят для ситуаций, где:

- Несколько полей данных должны отображаться в одном столбце GridView
- Данные лучше представить с помощью веб-элемент управления, а не обычного текста
- Результат зависит от базовых данных, таких как отображение метаданных или переформатирования данных

Хотя TemplateFields обеспечить большую степень гибкости при отрисовке DetailsView базовые данные, выходные данные DetailsView по-прежнему, в том числе немного boxy как каждое поле отображается в виде строки в HTML- `<table>`.

Элемент управления FormView предлагает повышенную гибкость в настройке выводимых данных. FormView не содержит поля, но вместо этого он только ряд шаблонов (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`и так далее). Мы рассмотрим способы использования FormView для обеспечения дополнительного управления отображении в обучении Далее.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было (Dan Jagers). Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](using-templatefields-in-the-gridview-control-cs.md)
> [Вперед](using-the-formview-s-templates-cs.md)
