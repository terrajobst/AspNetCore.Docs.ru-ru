---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: "Пользовательские кнопки в DataList и повторителя (C#) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы создадим интерфейс, который используется повторитель для получения списка категорий в системе, с помощью каждой категории, предоставляя кнопки, чтобы показать его associ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a072ae18bbb19d086eb825c6e72b68d40b2e429
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Пользовательские кнопки в DataList и повторителя (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) или [скачать PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> В этом учебнике мы создадим интерфейс, который используется повторитель для получения списка категорий в системе, с помощью каждой категории, предоставляя кнопку, чтобы отобразить соответствующие продукты с помощью элемента управления маркированный список.


## <a name="introduction"></a>Вступление

На протяжении последних семнадцать DataList и повторителя учебники мы хранять создан как примеры только для чтения и редактирования и удаления примеры. Чтобы упростить редактирование и удаление возможностей в DataList, мы добавили кнопки DataList s `ItemTemplate` , при щелчке вызвал обратную передачу и возникает событие DataList, соответствующий кнопку s `CommandName` свойство. Например, добавив в `ItemTemplate` с `CommandName` свойство редактирования значении DataList s `EditCommand` для срабатывания на обратной передачи; с `CommandName` удалить вызывает `DeleteCommand`.

Дополнительно на изменение и удаление кнопки, элементы управления DataList и повторителя могут также включать кнопок, элементов управления LinkButton и ImageButtons, при щелчке выполнять определенную настраиваемую логику на стороне сервера. В этом учебнике мы создадим интерфейс, который используется повторитель, чтобы вывести список категорий в системе. Для каждой категории повторителя будут содержаться кнопки, чтобы показать категории продуктов, связанные с помощью элемента управления маркированный список (см. рис. 1).


[![Щелкнув отображает ссылку продуктов Показать s категории продуктов в виде маркированного списка](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Рис. 1**: щелкнув отображает Показать связи продукты категории s продуктов в виде маркированного списка ([Просмотр полноразмерное изображение](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Шаг 1: Добавление пользовательской кнопки учебника веб-страницы

Прежде чем мы рассмотрим способы добавления настраиваемой кнопки позволяют s сначала занять некоторое время для создания страниц ASP.NET в данном проекте веб-сайта, он понадобится для этого учебника. Начните с добавления в новую папку с именем `CustomButtonsDataListRepeater`. Добавьте следующие две страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `CustomButtons.aspx`


![Добавление страниц ASP.NET для пользовательских кнопок руководств](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**На рисунке 2**: Добавление страницы ASP.NET для пользовательских кнопок руководств


Как и в других папках, `Default.aspx` в `CustomButtonsDataListRepeater` папки будут перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Этот пользовательский элемент управления, чтобы добавить `Default.aspx` , перетащив его из обозревателя решений на странице s представление конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Рис. 3**: добавление `SectionLevelTutorialListing.ascx` пользовательский элемент управления `Default.aspx` ([Просмотр полноразмерное изображение](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Наконец, добавления страниц, как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после разбиения по страницам и сортировка с DataList и повторителя `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элементы для редактирования, вставки и удаления учебники.


![Карта сайта теперь включает записи для пользовательских кнопок учебника](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Рис. 4**: карта сайта теперь включает записи для пользовательских кнопок учебника


## <a name="step-2-adding-the-list-of-categories"></a>Шаг 2: Добавление в список категорий

В этом учебнике мы должны установить повторителя, в которой перечислены все категории вместе с LinkButton Показать продукты, при нажатии отображает связанные категории s продуктов в виде маркированного списка. Разрешить s сначала создайте простой повторитель, перечислены категории в системе. Сначала откройте `CustomButtons.aspx` страницы в `CustomButtonsDataListRepeater` папки. Перетащите повторитель из области элементов в область конструктора и задайте его `ID` свойства `Categories`. Создайте новый элемент управления источником данных в смарт-теге s повторителя. В частности, создать новый элемент управления ObjectDataSource `CategoriesDataSource` , которая выбирает данные из `CategoriesBLL` класса s `GetCategories()` метод.


[![Настройка ObjectDataSource можно использовать метод GetCategories() класса CategoriesBLL s](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Рис. 5**: Настройка ObjectDataSource для использования `CategoriesBLL` класса s `GetCategories()` метод ([Просмотр полноразмерное изображение](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


В отличие от управления DataList, для которого по умолчанию Visual Studio создает `ItemTemplate` на основе источника данных, повторителя s шаблоны должны быть определены вручную. Кроме того, должно быть создано и редактировать декларативно шаблоны повторителя s (то есть отсутствует s не редактирование шаблонов параметра повторителя s смарт-тега).

Щелкните на вкладке "источник" в левом нижнем углу и добавьте `ItemTemplate` , отображает имя категории s в `<h3>` элемента и его описание в абзаце тег; включить `SeparatorTemplate` , отображает горизонтальную линию (`<hr />`) между каждым Категория. Также добавьте LinkButton с его `Text` значение свойства для отображения продуктов. После выполнения этих действий декларативная разметка s страница должна выглядеть следующим образом:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Рис. 6 показан страниц при просмотре через браузер. Отображается каждый название и описание категории. Показать продукты при нажатии кнопки, вызывает обратную передачу, но еще не выполняет никаких действий.


[![Каждая категория s имя и описание отображается, вместе с LinkButton Показать продуктов](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Рис. 6**: s каждой категории имя и описание отображается, вместе с LinkButton Показать продуктов ([Просмотр полноразмерное изображение](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Шаг 3: Выполнение серверную логику при Показать продуктов LinkButton нажата

Происходит каждый раз, когда нажимается кнопка, LinkButton или ImageButton внутри шаблона в DataList или повторителя обратная передача и s DataList или повторителя `ItemCommand` вызывается событие. В дополнение к `ItemCommand` событий DataList управления также может вызвать другой, более конкретные события, если кнопка s `CommandName` свойству присваивается одно из зарезервированных строк (удалять, изменять, "Отмена", обновление и выберите), но `ItemCommand` событие является *всегда* возникает.

При нажатии кнопки в DataList или повторителя зачастую необходимо передать какая кнопка была нажата (в случае, что может быть несколько кнопок в элементе управления, такие как оба изменения и кнопку «Удалить») и может быть некоторые дополнительные сведения (такие как значению ключа элемента, для которого была нажата). Кнопка, LinkButton и ImageButton предоставляют два свойства, значения которого будут переданы `ItemCommand` обработчик событий:

- `CommandName`Строка, обычно используются для определения каждой кнопки в шаблоне
- `CommandArgument`обычно используется для хранения значения некоторые поля данных, например значения первичного ключа

В этом примере набор LinkButton s `CommandName` свойство ShowProducts и привязки текущей записи s значения первичного ключа `CategoryID` для `CommandArgument` свойства, используя синтаксис привязки данных `CategoryArgument='<%# Eval("CategoryID") %>'`. После указания этих свойств, LinkButton s должен выглядеть следующим образом:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

При нажатии кнопки, обратная передача и s DataList или повторителя `ItemCommand` вызывается событие. Обработчик событий передается кнопку s `CommandName` и `CommandArgument` значения.

Создайте обработчик событий для повторителя s `ItemCommand` событий и обратите внимание на второй параметр, переданный в обработчик событий (с именем `e`). Этот второй параметр имеет тип [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) и имеет следующие четыре свойства:

- `CommandArgument`значение нажатой кнопке s `CommandArgument` свойство
- `CommandName`значение кнопки s `CommandName` свойство
- `CommandSource`ссылку на элемент управления button, который был щелкнут
- `Item`ссылку на [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , содержащий кнопки, которая была нажата; каждая запись, привязанный к повторителя представляется в виде`RepeaterItem`

С момента выбранной категории s `CategoryID` переданными в виде `CommandArgument` свойства, мы можем получить набор продукты, связанные с выбранной категории в `ItemCommand` обработчика событий. Эти продукты затем могут быть привязаны к элементу управления BulletedList в `ItemTemplate` (который мы хранять еще для добавления). Все остается, затем — добавление маркированный список ссылок на него в `ItemCommand` обработчик событий и привязать к нему набор продуктов для выбранной категории, мы исследуем в этом выпуске на шаге 4.

> [!NOTE]
> Элемент управления DataList s `ItemCommand` обработчик событий передается объект типа [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), который предоставляет те же четыре свойства, как `RepeaterCommandEventArgs` класса.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Шаг 4: Отображение s выбранной категории продуктов в виде маркированного списка

Продукты выбранной категории s может отображаться в повторителя s `ItemTemplate` с помощью любое количество элементов управления. Нам удалось добавить другой вложенные повторителя, DataList, DropDownList, GridView и т. д. Поскольку мы хотим для отображения продуктов в виде маркированного списка, менее, мы будем использовать элемент управления маркированный список. Возврат к `CustomButtons.aspx` s декларативная разметка страницы, добавить элемент управления BulletedList `ItemTemplate` после LinkButton Показать продуктов. Набор BulletedLists s `ID` для `ProductsInCategory`. Маркированный список отображает значение поля данных, указанного с помощью `DataTextField` свойства; поскольку этот элемент управления будет иметь сведения о продукте привязанным к нему, установить `DataTextField` свойства `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

В `ItemCommand` обработчика событий, ссылаются на этот элемент управления с помощью `e.Item.FindControl("ProductsInCategory")` и привязать их к набору продукты, связанные с выбранной категории.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Перед выполнением любого действия в `ItemCommand` обработчика событий, он s, разумным шагом будет сначала проверьте значение входящего `CommandName`. Поскольку `ItemCommand` обработчик событий применяется при *любой* кнопки, если используется шаблон нескольких кнопок `CommandName` значение для распознавания какое действие необходимо выполнить. Проверка `CommandName` Вот получается, так как у нас имеется только одна кнопка, но это хорошая привычка формы. Далее, `CategoryID` выбранной категории извлекается из `CommandArgument` свойство. Элемент управления маркированный список в шаблоне является ссылка на, а также к результаты `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод.

В предыдущих учебниках, используемые кнопки в DataList, такие как [Обзор редактирование и удаление данных в DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), мы определили значения первичного ключа данного элемента через `DataKeys` коллекции. Хотя этот подход хорошо работает с DataList, не предоставляет достаточно повторителя `DataKeys` свойство. Вместо этого необходимо использовать альтернативный подход для предоставления значения первичного ключа, например через кнопку s `CommandArgument` свойства или путем присвоения значения первичного ключа для скрытых веб-управления Label в шаблоне и чтения его значения в `ItemCommand`при помощи команды `e.Item.FindControl("LabelID")`.

После завершения `ItemCommand` обработчика событий, теперь пора проверить эту страницу в браузере. Как показано на рис. 7, нажав кнопку Показать продукты ссылка вызывает обратную передачу и отображает для выбранной категории продуктов в маркированный список. Кроме того Обратите внимание на то что остается эти сведения о продукте, даже при нажатии на другие категории продуктов Показать ссылки.

> [!NOTE]
> Если вы хотите изменить поведение этого отчета таким образом, что только одна категория s продукты отображаются одновременно, просто установите элемент управления маркированный список s `EnableViewState` свойства `False`.


[![Маркированный список используется для отображения продуктов выбранной категории](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Рис. 7**: маркированный список используется для отображения продуктов выбранной категории ([Просмотр полноразмерное изображение](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Сводка

Элементы управления DataList и повторителя может включать любое количество кнопок, элементов управления LinkButton и ImageButtons в своих шаблонов. Такие кнопки при щелчке вызывает обратную передачу и вызвать `ItemCommand` событий. Чтобы связать настраиваемого действия на стороне сервера с помощью нажатия кнопки, создайте обработчик событий для `ItemCommand` события. В этот обработчик событий сначала проверьте входящего `CommandName` значение, чтобы определить, какая кнопка была нажата. Дополнительные сведения при необходимости может предоставляться через кнопку s `CommandArgument` свойство.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было Патерсона Деннис. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Вперед](custom-buttons-in-the-datalist-and-repeater-vb.md)
