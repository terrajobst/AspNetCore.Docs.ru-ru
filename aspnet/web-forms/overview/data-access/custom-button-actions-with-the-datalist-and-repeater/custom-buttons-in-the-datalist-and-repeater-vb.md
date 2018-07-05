---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Настраиваемые кнопки в элементах управления DataList и Repeater (VB) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы создадим интерфейс, который использует элемент управления Repeater для категорий в системе, с каждой категорией, предоставляя кнопки, чтобы показать его associ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 7dc738e22f7670aa3eeb07d69888a4df47c1fc82
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371588"
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Настраиваемые кнопки в элементах управления DataList и Repeater (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) или [скачать PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> В этом руководстве мы создадим интерфейс, который использует элемент управления Repeater для категорий в системе, с каждой категорией, предоставляя кнопку, чтобы отобразить соответствующие продукты с помощью элемента управления BulletedList.


## <a name="introduction"></a>Вступление

В предыдущих учебных семнадцати элементов управления DataList и Repeater мы ve создан как примеры только для чтения и редактирования и удаления примеры. Чтобы упростить редактирование и удаление возможностей в элемент управления DataList, мы добавили кнопки элемента управления DataList s `ItemTemplate` , при нажатии вызвал обратную передачу и возникает событие элемента управления DataList, соответствующий кнопки s `CommandName` свойство. Например, добавление кнопки для `ItemTemplate` с `CommandName` свойство редактирования значении DataList s `EditCommand` срабатывание при обратной передаче; с `CommandName` удалить вызывает `DeleteCommand`.

Кроме того чтобы изменить или удалить кнопки, элементы управления DataList и Repeater можно также включать кнопок, элементов управления LinkButton или ImageButtons, при нажатии выполняют определенную настраиваемую логику на стороне сервера. В этом руководстве мы создадим интерфейс, который использует элемент управления Repeater, чтобы получить список категорий в системе. Для каждой категории Repeater будет содержать кнопку, чтобы показать категории продуктов, связанные с помощью элемента управления BulletedList (см. рис. 1).


[![Щелкнув ссылку дисплеев продуктов Показать продукты s категории в маркированном списке](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Рис. 1**: щелчок отображает Показать ссылку продукты категории s продуктов в маркированном списке ([Просмотр полноразмерного изображения](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Шаг 1: Добавление пользовательской кнопки учебника веб-страниц

Прежде чем мы рассмотрим добавление пользовательской кнопки, позвольте s уделим несколько минут созданию страниц ASP.NET в нашем проекте веб-сайта, что нам понадобится для этого руководства. Начните с добавления новой папки с именем `CustomButtonsDataListRepeater`. Добавьте следующие две страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `CustomButtons.aspx`


![Добавление страниц ASP.NET для пользовательских кнопок руководств](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Рис. 2**: Добавление страниц ASP.NET для пользовательских кнопок руководств


Как и в других папках, `Default.aspx` в `CustomButtonsDataListRepeater` папку перечислит учебные курсы в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Добавьте данный пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s режиме конструктора.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Рис. 3**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


Наконец, добавьте страницы как записи, чтобы `Web.sitemap` файл. В частности, добавьте следующую разметку после разбиения по страницам и сортировка с помощью элементов управления DataList и Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элементы для редактирования, вставки и удаления учебных курсов.


![Карта узла теперь есть запись для пользовательских кнопок учебника](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Рис. 4**: карта узла теперь есть запись для пользовательских кнопок учебника


## <a name="step-2-adding-the-list-of-categories"></a>Шаг 2: Добавление списка категорий

В этом руководстве, нам нужно создать элемент управления Repeater, в которой перечислены все категории с указанием LinkButton Показать продукты, при нажатии отображает связанной категории s продуктов в виде маркированного списка. Позвольте s сначала создать простой Repeater, в котором выводятся категории в системе. Сначала откройте `CustomButtons.aspx` странице в `CustomButtonsDataListRepeater` папку. Перетащите элемент управления Repeater из области элементов в конструктор и задайте его `ID` свойства `Categories`. Создайте новый элемент управления источника данных в смарт-теге элемента управления Repeater s. В частности, создать новый элемент управления ObjectDataSource с именем `CategoriesDataSource` , который выбирает данные из `CategoriesBLL` класс s `GetCategories()` метод.


[![Настройте элемент ObjectDataSource для использования метода GetCategories() класса CategoriesBLL s](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Рис. 5**: Настройка ObjectDataSource для использования `CategoriesBLL` класс s `GetCategories()` метод ([Просмотр полноразмерного изображения](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


В отличие от элемента управления DataList, для которого Visual Studio создает по умолчанию `ItemTemplate` в зависимости от источника данных элемента управления Repeater s шаблоны должны быть определены вручную. Кроме того, необходимо создавать и редактировать декларативно шаблонов элемента управления Repeater s (то есть здесь s не Правка шаблонов параметра в смарт-теге элемента управления Repeater s).

Щелкните на вкладке "источник" в левом нижнем углу и добавьте `ItemTemplate` , отображающий имя категории s в `<h3>` элемент и его описание в абзаце тега; включает `SeparatorTemplate` , отображающий горизонтальной чертой (`<hr />`) между каждым Категория. Также добавьте LinkButton с его `Text` свойство для отображения продуктов. После выполнения этих действий декларативная разметка s страница должна выглядеть следующим образом:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Рис. 6 показана страница в обозревателе. Отображается каждый имя и описание категории. Показать продукты при нажатии кнопки, вызывает обратную передачу, но еще не выполняет никаких действий.


[![Каждой категории — имя и описание отображается, вместе с LinkButton Показать продукты](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Рис. 6**: Каждая категория — имя и описание отображается, вместе с LinkButton Показать продукты ([Просмотр полноразмерного изображения](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Шаг 3: Выполнение серверных логики при Показать продукты LinkButton нажата

Каждый раз, когда нажата кнопка, LinkButton или ImageButton в шаблоне элемента управления DataList или Repeater, происходит обратная передача и s DataList или Repeater `ItemCommand` вызывает событие. В дополнение к `ItemCommand` событий, DataList, элемент управления также может вызывать другой, более конкретное событие, если кнопка s `CommandName` свойству присваивается одно из зарезервированных строк (удаления, редактирования, Отмена, Update или Select), но `ItemCommand` событие является *всегда* инициируется.

При нажатии кнопки в элементе управления DataList или Repeater, зачастую необходимо передать вдоль какая кнопка была нажата (в случае, что может быть несколько кнопок в элементе управления, такие как режимах редактирования и кнопки "Удалить"), а также возможно, некоторые дополнительные сведения (такие как значению ключа элемента, для которого была нажата). Кнопка, LinkButton и ImageButton предоставляют два свойства, значения которых передаются `ItemCommand` обработчик событий:

- `CommandName` Строка, обычно используется для идентификации каждой кнопки в шаблоне
- `CommandArgument` обычно используется для хранения значения некоторые поля данных, таких как значение первичного ключа

В этом примере набор LinkButton s `CommandName` свойство ShowProducts и привязки текущей записи s значение первичного ключа `CategoryID` для `CommandArgument` свойства, используя синтаксис привязки данных `CategoryArgument='<%# Eval("CategoryID") %>'`. После указания этих двух свойств, декларативный синтаксис s LinkButton должен выглядеть следующим образом:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

При нажатии кнопки, происходит обратная передача и s DataList или Repeater `ItemCommand` вызывает событие. Обработчик событий передается кнопки s `CommandName` и `CommandArgument` значения.

Создайте обработчик событий для элемента управления Repeater s `ItemCommand` событий и запишите второго параметра, передаваемого в обработчик событий (с именем `e`). Этот второй параметр имеет тип [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) и имеет следующие четыре свойства:

- `CommandArgument` значение нажатой кнопки s `CommandArgument` свойство
- `CommandName` значение кнопки s `CommandName` свойство
- `CommandSource` ссылка на элемент управления button, которая была нажата
- `Item` ссылку на [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , содержащий кнопку, которая была нажата; в каждой записи, привязанного к элементу управления Repeater представляется в виде `RepeaterItem`

С момента выбранной категории s `CategoryID` переданной через `CommandArgument` свойство, мы получаем набор продуктов, связанных с выбранной категорией в `ItemCommand` обработчик событий. Затем эти продукты можно привязать к элементу управления BulletedList в `ItemTemplate` (какие мы ve еще для добавления). Все остается, затем — добавить маркированный список, ссылаются на него в `ItemCommand` обработчик событий и привязать к ней набора продуктов выбранной категории, который будет разобран на шаге 4.

> [!NOTE]
> Элемент управления DataList s `ItemCommand` обработчику события передаются в объект типа [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), который предлагает те же четыре свойства, что `RepeaterCommandEventArgs` класса.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Шаг 4: Отображение s продукты выбранной категории в маркированном списке

Продукты выбранной категории s могут отображаться в элементе Repeater s `ItemTemplate` используя любое количество элементов управления. Нам удалось добавить другой вложенной Repeater, DataList, DropDownList, GridView и т. д. Поскольку мы хотим отобразить продукты в виде маркированного списка, однако мы будем использовать элемент управления BulletedList. Возвращаясь к `CustomButtons.aspx` s декларативная разметка страницы, добавьте элемент управления BulletedList для `ItemTemplate` после LinkButton Показать продукты. Набор BulletedLists s `ID` для `ProductsInCategory`. Маркированный список отображает значение поля данных, указанной с помощью `DataTextField` свойство; так как этот элемент управления будет привязан к нему, задайте сведения о продукте `DataTextField` свойства `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

В `ItemCommand` обработчик событий, ссылаются на этот элемент управления с помощью `e.Item.FindControl("ProductsInCategory")` и привяжите его к набору продуктов, связанных с выбранной категорией.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Прежде чем выполнять любое действие в `ItemCommand` обработчик событий, он s, разумно сначала проверьте значение входящего `CommandName`. Так как `ItemCommand` обработчик событий применяется при *любой* кнопки, при наличии нескольких кнопок в использовании шаблонов `CommandName` значение определить, какое действие следует предпринять. Проверка `CommandName` Вот комбинированного, так как у нас есть только одна кнопка, но это хорошая привычка формы. Далее, `CategoryID` выбранной категории извлекается из `CommandArgument` свойство. Элемент управления BulletedList в шаблоне ссылка, а также к результаты `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод.

В предыдущих учебных курсах, которые используются кнопки в элемент управления DataList, такие как [обзор редактирования и удаления данных в DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), мы определили значение первичного ключа с помощью данного элемента `DataKeys` коллекции. Хотя этот подход хорошо работает с DataList, Repeater не имеет `DataKeys` свойства. Вместо этого необходимо использовать альтернативный подход для предоставления значения первичного ключа, например, нажмите кнопку "s `CommandArgument` свойства или путем присвоения значения первичного ключа для скрытого элемента управления Label Web в шаблоне и чтения его значения в `ItemCommand`обработчика событий с помощью `e.Item.FindControl("LabelID")`.

После завершения `ItemCommand` обработчик событий, Отвлекитесь и проверьте страницу в браузере. Как показано на рис. 7, нажав кнопку Показать продукты ссылку вызывает обратную передачу и отображает продукты для выбранной категории в маркированный список. Кроме того внимание, что данная информация остается, даже если выбраны другие категории продуктов, Показать ссылки.

> [!NOTE]
> Если вы хотите изменить поведение этого отчета таким образом, что только одна категория s продукты отображаются одновременно, просто задать элемент управления BulletedList s `EnableViewState` свойства `False`.


[![Маркированный список используется для отображения продуктов выбранной категории](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Рис. 7**: маркированный список используется для отображения продуктов выбранной категории ([Просмотр полноразмерного изображения](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Сводка

Элементы управления DataList и Repeater может включать любое количество кнопок, элементов управления LinkButton или ImageButtons внутри их шаблоны. При нажатии эти кнопки вызывает обратную передачу и вызывать `ItemCommand` событий. Чтобы связать пользовательское действие на стороне сервера с помощью нажатия кнопки, создайте обработчик событий для `ItemCommand` событий. В этом обработчике сперва проверяется входящее значение `CommandName` значение, чтобы определить, какая кнопка была нажата. Дополнительные сведения при необходимости может предоставляться через кнопку s `CommandArgument` свойство.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был Dennis Patterson. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](custom-buttons-in-the-datalist-and-repeater-cs.md)
