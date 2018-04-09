---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Главного и подчиненного представлений с помощью маркированного списка основных записей с помощью DataList сведения (C#) | Документы Microsoft
author: rick-anderson
description: В этом учебнике мы будем сжатие двухстраничный главного и подчиненного представлений отчета предыдущего учебника на одной странице отображения маркированного списка имен категорий на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c041c352c379dc1d3c0f13013e7e323faa500912
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Главного и подчиненного представлений с помощью маркированного списка основных записей с помощью DataList сведения (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) или [скачать PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> В этом учебнике мы будем сжатие двухстраничный главного и подчиненного представлений отчета предыдущего учебника в одну страницу, отображение маркированный список имен категорий в левой части экрана и выбранной категории продуктов в правой части экрана.


## <a name="introduction"></a>Вступление

В [предыдущего учебника](master-detail-filtering-acess-two-pages-datalist-cs.md) мы рассмотрели для разделения главного и подчиненного представлений отчета на двух страницах. На главной странице мы использовали управления повторителем для подготовки к просмотру маркированный список категорий. Имя каждой категории был гиперссылку, при нажатии будет take пользователя на страницу сведений, где DataList два столбца показал этих продуктов, принадлежащих выбранной категории.

В этом учебнике мы будем сжатие учебника две страницы на одной странице, отображение маркированный список имен категорий в левой части экрана с к просмотру в виде LinkButton имена категорий. Выбрав одну из имени категории элементов управления LinkButton вызывает обратную передачу и привязывает s выбранной категории продуктов DataList два столбца в правой части экрана. Помимо отображения имена категорий s, повторителя слева показывает, сколько всего продуктов для данной категории (см. рис. 1).


[![В левой части экрана отображаются имя категории и общее число продуктов](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Рис. 1**: s категории имя и общее число продуктов отображаются в левой части ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Шаг 1: Отображение повторитель в левой части экрана

В этом учебнике мы должны маркированный список категорий отображаются слева от выбранной категории продуктов s. Можно разместить содержимое веб-страниц с помощью стандартных тегов абзаца элементы HTML, неразрывные пробелы `<table>` s и т. д или использованием методик, каскадные таблицы стилей (CSS). Всех наших учебниках сих использовали CSS методы для позиционирования. Когда мы создали пользовательского интерфейса навигации в нашей главной страницы в [главные страницы и переходов](../introduction/master-pages-and-site-navigation-cs.md) учебника мы использовали *абсолютное позиционирование*, указывающим смещение точный пикселей для навигации список и основного содержимого. Кроме того, можно использовать CSS для размещения одного элемента вправо или влево от другой по *плавающей*. У нас есть маркированный список категорий, с плавающей запятой повторителя слева от DataList отображаются слева от выбранной категории продуктов s

Откройте `CategoriesAndProducts.aspx` страницу `DataListRepeaterFiltering` папки и добавить на страницу повторитель и DataList. Задать повторителя s `ID` для `Categories` и DataList s для `CategoryProducts`. Перейдите в представление источника и поместить элементы управления повторителем и DataList внутри их собственных `<div>` элементов. То есть, заключите повторителя в `<div>` элемент первой и затем DataList в отдельном `<div>` элемента непосредственно после повторителя. На этом этапе разметке должен выглядеть примерно следующее:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

В число с плавающей запятой повторителя слева от DataList, необходимо использовать `float` атрибут стиля CSS, следующим образом:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;` Смещает первый `<div>` элемента слева от второго. `width` И `padding-right` параметры указывают первый `<div>` s `width` и размер внутреннего отступа между добавляется `<div>` содержимое элемента s и ее правого поля. Дополнительные сведения о перемещаемые элементы в CSS извлечь [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Вместо того чтобы указать параметр style напрямую через первый `<p>` элемент s `style` атрибута, позволяя вместо этого создайте новый класс CSS в s `Styles.css` с именем `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Затем мы можно заменить `<div>` с `<div class="FloatLeft">`.

После добавления класса CSS и настройки разметки в `CategoriesAndProducts.aspx` перейдите в конструктор. Вы увидите повторителя плавающей слева от DataList (обычно справа Теперь оба так же как серый полей с момента мы хранять еще, чтобы настроить их источники данных или шаблоны).


[![Повторителя перемещается влево DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**На рисунке 2**: повторителя перемещается влево DataList ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Шаг 2: Определение количества продуктов для каждой категории

С помощью повторителя и DataList s вокруг готова мы готов для привязки данных категории повторителя управлять. Тем не менее как показывает маркированный список категорий на рисунке 1, помимо s имена категорий мы также должны отображать количество продуктов, связанные с категорией. Для доступа к этой информации мы можно:

- **Определите класс кода программной s страницы ASP.NET этих сведений.** Получает определенный *`categoryID`* мы можно определить число соответствующих продуктов, вызвав `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод. Этот метод возвращает `ProductsDataTable` которого `Count` свойство указывает, сколько `ProductsRow` существует s, который является количество продуктов для указанного *`categoryID`*. Мы можем создать `ItemDataBound` обработчика событий, для каждой категории, привязанный к повторителя вызывает повторителя `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод и включает его счетчик в выходных данных.
- **Обновление `CategoriesDataTable` типизированного набора данных для включения `NumberOfProducts` столбца.** Затем можно обновить `GetCategories()` метод в `CategoriesDataTable` включить эту информацию, или можно также оставить `GetCategories()` как- и создать новый `CategoriesDataTable` метод с именем `GetCategoriesAndNumberOfProducts()`.

Разрешить просмотр оба этих метода s. Первый подход — проще, так как мы не хотите t необходимость обновить уровень доступа к данным; Однако он требует дополнительные связи с базой данных. Вызов `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод в `ItemDataBound` обработчик событий добавляет вызова дополнительные базы данных для каждой категории отображается в Повторителе. С помощью этого метода есть *N* + вызовы 1 базы данных, где *N* — количество категорий, отображаемых в Повторителе. Во втором подходе возвращается число продуктов со сведениями о каждой категории из `CategoriesBLL` класса s `GetCategories()` (или `GetCategoriesAndNumberOfProducts()`) метода, тем самым возникающие только одно обращение к базе данных.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Определение количества продуктов в обработчике событий ItemDataBound

Определение числа продуктов в каждой категории в повторителя s `ItemDataBound` обработчик событий не требует каких-либо изменений наших существующего уровня доступа к данным. Можно внести все изменения непосредственно в `CategoriesAndProducts.aspx` страницы. Начните с добавления новых ObjectDataSource с именем `CategoriesDataSource` через повторителя s смарт-тег. Настройте `CategoriesDataSource` ObjectDataSource, так что он извлекает данные из `CategoriesBLL` класса s `GetCategories()` метод.


[![Настройка ObjectDataSource с помощью класса CategoriesBLL s GetCategories()-метод](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Рис. 3**: Настройка ObjectDataSource для использования `CategoriesBLL` класса s `GetCategories()` метод ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Каждый элемент в `Categories` повторителя необходимо активную и, при щелчке вызывают `CategoryProducts` DataList для отображения этих продуктов для выбранной категории. Это можно сделать, делая гиперссылку, связав этой же странице каждой категории (`CategoriesAndProducts.aspx`), но передав `CategoryID` через строку запроса, подобно мы видели в предыдущем учебнике. Преимущество такого подхода — что страницы, на которой продукты конкретной категории s можно с закладками и проиндексирован поисковой системы.

Кроме того можно сделать каждой категории LinkButton, который является подход, который будет использоваться для этого учебника. LinkButton подготавливается к просмотру в браузере пользователя s как гиперссылку, но после щелчка вызывает обратную передачу; При обратной передаче DataList s ObjectDataSource необходимо обновить для отображения этих продуктов, принадлежащих выбранной категории. В этом учебнике гиперссылку делает более понятны, чем при использовании LinkButton; Тем не менее могут существовать другие сценарии, где с помощью LinkButton более выгодным. А подход гиперссылка будет идеальным для этого примера, позволяют s вместо просматривать с помощью элемента управления LinkButton. Как будет видно, с помощью LinkButton возникают определенные проблемы, которые бы в противном случае не возникает с гиперссылкой. Таким образом с помощью LinkButton в этом учебнике будет выделять этих проблем и решений для обеспечения этих сценариев, где мы хотим использовать LinkButton вместо гиперссылки.

> [!NOTE]
> Рекомендуется повторить занятия по использованию элемента управления HyperLink или `<a>` элемент вместо LinkButton.


Приведенная ниже разметка показано использование декларативного синтаксиса для повторителя и ObjectDataSource. Обратите внимание, шаблоны s повторителя отображения маркированного списка в качестве LinkButton каждый элемент.


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> В этом учебнике повторителя должен иметь свое состояние представления включено (Обратите внимание, заменяют `EnableViewState="False"` из декларативного синтаксиса повторителя s). На шаге 3 будут созданы обработчик событий для повторителя s `ItemCommand` событий, в котором мы будем обновление DataList s ObjectDataSource s `SelectParameters` коллекции. Повторителя s `ItemCommand`, но при этом выиграл пожара t, если состояние просмотра отключено. В разделе [Stumper вопроса ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) и [решения](http://scottonwriting.net/sowBlog/posts/1268.aspx) Дополнительные сведения о том, почему состояние просмотра должна быть включена для повторителя s `ItemCommand` срабатывание события.


ImageButton с `ID` значение свойства `ViewCategory` не поддерживает его `Text` набор свойств. Если мы хотели только отображаемое имя категории, мы бы задайте свойству Text декларативно, через синтаксис привязки данных следующим образом:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Тем не менее, мы хотим Показать имя категории s *и* количество продуктов, относящихся к этой категории. Эти сведения можно получить из повторителя s `ItemDataBound` обработчик события путем вызова `ProductBLL` класса s `GetCategoriesByProductID(categoryID)` метод и определить, сколько записей в результате `ProductsDataTable`, как следующий код иллюстрирует:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Мы начинаем с гарантия того, что мы re работа с элементом данных (один которого `ItemType` — `Item` или `AlternatingItem`) и затем ссылаться на `CategoriesRow` экземпляр, который привязан только к текущему `RepeaterItem`. Затем мы определить количество продуктов для данной категории, создав экземпляр `ProductsBLL` класса, вызвав его `GetCategoriesByProductID(categoryID)` метод и определяя необходимое количество записей, возвращаемых с помощью `Count` свойство. Наконец `ViewCategory` LinkButton в ItemTemplate является ссылки и его `Text` свойству *CategoryName* (*NumberOfProductsInCategory*), где  *NumberOfProductsInCategory* форматируется в виде числа с избытком.

> [!NOTE]
> Кроме того, может добавлена *форматирование функция* классу фонового кода ASP.NET страницы s, который принимает категории s `CategoryName` и `CategoryID` значения и возвращает `CategoryName` сцепляется с количеством продукты в категории (как определено вызовом `GetCategoriesByProductID(categoryID)` метода). Результаты форматирования функции может быть назначен декларативно для s LinkButton свойства Text, нет необходимости в `ItemDataBound` обработчика событий. Ссылаться на [с помощью TemplateFields в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) или [форматирование DataList и повторителя после данных](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) учебники Дополнительные сведения об использовании функции форматирования.


После добавления этого обработчика событий, теперь пора проверить страницу через браузер. Обратите внимание на то, как каждой категории отображается в виде маркированного списка отображения имени категории s и количество продуктов, связанные с категорией (см. рис. 4).


[![Каждая категория s имя и число продуктов отображаются](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Рис. 4**: s каждой категории имя и число продуктов отображаются ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Обновление`CategoriesDataTable`и`CategoriesTableAdapter`для включения количество продуктов для каждой категории

Вместо определения количества продуктов в каждой категории, как с привязкой к повторителя, мы может упростить этот процесс, перемещая `CategoriesDataTable` и `CategoriesTableAdapter` уровне доступа к данным, чтобы включить эти сведения в собственном коде. Чтобы добиться этого, необходимо добавить новый столбец в `CategoriesDataTable` для хранения числа соответствующих продуктов. Чтобы добавить новый столбец в таблицу данных, откройте типизированного набора данных (`App_Code\DAL\Northwind.xsd`), щелкните правой кнопкой мыши на таблицу данных для изменения и нажмите кнопку Добавить, и столбец. Добавить новый столбец в `CategoriesDataTable` (см. рис. 5).


[![Добавить новый столбец CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Рис. 5**: добавить новый столбец в `CategoriesDataSource` ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Это будет добавлен новый столбец с именем `Column1`, которые можно изменить, просто введя другое имя. Переименовать этот новый столбец в `NumberOfProducts`. Теперь нам нужны для настройки этого свойства столбец s. Щелкните новый столбец и перейдите в окно «Свойства». Изменить столбец s `DataType` свойство из `System.String` для `System.Int32` и задайте `ReadOnly` свойства `True`, как показано на рис. 6.


![Задать свойства только для чтения нового столбца и тип данных](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Рис. 6**: задать `DataType` и `ReadOnly` свойства нового столбца


Хотя `CategoriesDataTable` теперь имеет `NumberOfProducts` столбца, его значение не задано с помощью любой из соответствующих запросов TableAdapter s. Корпорация Майкрософт может обновлять `GetCategories()` метод для возврата этих сведений в том случае, если мы хотим такие сведения, возвращаемые каждый раз, чтобы получить сведения о категории. Если, однако нам нужно взять количество связанных продуктов для категорий в редких случаях (например, как только в этом учебнике), то можно оставить `GetCategories()` как- и создать новый метод, который возвращает эту информацию. S позволяют использовать такой подход, создание нового метода с именем `GetCategoriesAndNumberOfProducts()`.

Чтобы добавить этот новый `GetCategoriesAndNumberOfProducts()` метод, щелкните правой кнопкой мыши `CategoriesTableAdapter` и выберите новый запрос. Это дает вам TableAdapter запроса мастер настройки, который мы хранить, используется несколько раз в предыдущих учебниках вверх. Для этого метода запустите мастер, указывая, что в запросе используется инструкция SQL ad-hoc, возвращает строки.


[![Создайте метод, с помощью инструкции SQL Ad-Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Рис. 7**: создайте метод с помощью инструкции SQL Ad-Hoc ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![Инструкция SQL возвращает строки](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Рис. 8**: возвращает строки инструкции SQL ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


На следующем экране мастера предлагает нам запрос для использования. Для возвращения каждой категории s `CategoryID`, `CategoryName`, и `Description` поля, и количество продуктов, связанные с категорией, воспользуйтесь следующим `SELECT` инструкции:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Укажите запрос для использования](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Рис. 9**: укажите запрос для использования ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Обратите внимание, что вложенный запрос, который вычисляет количество продуктов, связанные с категорией псевдоним `NumberOfProducts`. Это совпадение имен приводит значения, возвращенного этот вложенный запрос, должны быть связаны с `CategoriesDataTable` s `NumberOfProducts` столбца.

После ввода этого запроса, последним шагом является выбор имя для нового метода. Используйте `FillWithNumberOfProducts` и `GetCategoriesAndNumberOfProducts` для заполнения таблицы данных и Возврат DataTable, шаблоны, соответственно.


[![Имя нового FillWithNumberOfProducts методы TableAdapter s и GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Рис. 10**: имя s новый адаптер таблицы методы `FillWithNumberOfProducts` и `GetCategoriesAndNumberOfProducts` ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


На этом этапе уровень доступа к данным был расширен для включения количество продуктов по категориям. Поскольку слоя представления перенаправляет все вызовы DAL через отдельный уровень бизнес-логики необходимо добавить соответствующий `GetCategoriesAndNumberOfProducts` метод `CategoriesBLL` класса:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

DAL и уровень бизнес-ЛОГИКИ завершения, мы re готов для привязки данных `Categories` повторителя в `CategoriesAndProducts.aspx`! Если хранить вы уже создали ObjectDataSource для повторителя из определение число продуктов в `ItemDataBound` раздела с обработчиком событий, удалить этот элемент управления ObjectDataSource и повторителя s `DataSourceID` свойства параметра; также жизнь без проводов Повторителя s `ItemDataBound` событий из обработчика событий, удаляя `Handles Categories.OnItemDataBound` синтаксиса в классе фонового кода ASP.NET.

С помощью повторителя обратно в исходное состояние, добавить новый элемент управления ObjectDataSource с именем `CategoriesDataSource` через повторителя s смарт-тег. Настройка ObjectDataSource для использования `CategoriesBLL` класса, но вместо использования `GetCategories()` в методе, имеет он используется `GetCategoriesAndNumberOfProducts()` вместо (см. рис. 11).


[![Настройка ObjectDataSource можно использовать метод GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Рис. 11**: Настройка ObjectDataSource для использования `GetCategoriesAndNumberOfProducts` метод ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Затем обновите `ItemTemplate` , чтобы LinkButton s `Text` свойство назначается декларативно с помощью синтаксиса привязки данных и включает в себя `CategoryName` и `NumberOfProducts` полей данных. Полный декларативная разметка для повторителя и `CategoriesDataSource` ObjectDataSource выполните:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

Выходные данные подготовки к просмотру в обновлении DAL для включения `NumberOfProducts` столбец содержит то же, как с помощью `ItemDataBound` подход обработчик событий (ссылать на рис. 4 для просмотра на экране снимок повторителя, в котором отображаются имена категорий и количество продуктов).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Шаг 3: Отображение выбранной категории продуктов s

На этом этапе у нас есть `Categories` повторителя со списком категорий, а также количество продуктов в каждой категории. Повторителя использует LinkButton для каждой категории, что при нажатии вызывает обратную передачу, с которой указывают мы должны отображать эти продукты для выбранной категории в `CategoryProducts` DataList.

Одна из проблем с выходом нам описана настройка DataList отобразить только те продукты, для выбранной категории. В [главный/Detail, с помощью выбираемого основного элемента GridView с DetailsView сведения](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) учебника мы узнали, как построить GridView, строки которого может быть выбрана, с выбранной строки s сведений, отображаемых в элементе управления DetailsView на одной странице. GridView s ObjectDataSource возвращаются сведения обо всех приложениях с помощью `ProductsBLL` s `GetProducts()` метод при DetailsView s ObjectDataSource получить сведения об использовании выбранного продукта `GetProductsByProductID(productID)` метод. *`productID`* Декларативно указано значение параметра, связав ее со значением GridView s `SelectedValue` свойство. К сожалению, нет повторителя `SelectedValue` свойство и не может выступать в качестве источника параметра.

> [!NOTE]
> Это одна из этих проблем, появляющихся при использовании LinkButton в повторитель. Мы использовали гиперссылки для передачи в `CategoryID` через строку запроса вместо этого можно использовать это поле QueryString как источник для значения параметра s.


Прежде чем мы волноваться по поводу отсутствия `SelectedValue` свойства для повторителя, однако let s сначала привязать элемент управления DataList ObjectDataSource и укажите его `ItemTemplate`.

Смарт-теге DataList s необязательно, чтобы добавить новый элемент управления ObjectDataSource с именем `CategoryProductsDataSource` и настройте его для использования `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод. Поскольку в этом учебнике DataList предлагает интерфейс только для чтения, вы можете задать раскрывающихся списков в инструкциях INSERT, UPDATE и удаление вкладок (нет).


[![Настройка ObjectDataSource использовать метод GetProductsByCategoryID(categoryID) класса ProductsBLL s](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Рис. 12**: Настройка ObjectDataSource для использования `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Поскольку `GetProductsByCategoryID(categoryID)` метод требует входной параметр (*`categoryID`*), мастер настройки источника данных позволяет указать источник параметра s. Категории перечислены в элементе управления GridView или DataList, d задается параметр исходного раскрывающегося списка для управления и ControlID в `ID` данных веб-элемента управления. Однако, поскольку отсутствует повторителя `SelectedValue` свойства не может использоваться как источник параметра. Если установить флажок, вы найдете список ControlID раскрывающийся список содержит только один элемент управления `ID``CategoryProducts`, `ID` элемента управления DataList.

На данном этапе задайте список раскрывающегося списка параметров источника нет. Мы получим программно назначение этого параметра значение, при нажатии LinkButton в Повторителе категории.


[![Выполните не указан источник параметров для categoryID параметра](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Рис. 13**: не указан источник параметров для *`categoryID`* параметра ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


После завершения работы мастера настройки источника данных Visual Studio автоматически создает элемент управления DataList s `ItemTemplate`. Замените это значение по умолчанию `ItemTemplate` с шаблоном мы используется в предыдущем учебнике; Кроме того, задать DataList s `RepeatColumns` равным 2. После внесения этих изменений декларативная разметка для DataList и связанные методы должны выглядеть следующим образом:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

В настоящее время `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* никогда не установлено, поэтому продукты не отображаются при просмотре страницы. Нам нужно выполнить будет иметь значение параметра, установленное на основе `CategoryID` выбранной категории в Повторителе. Это возникают две проблемы: во-первых, как мы определяем при LinkButton в повторителя s `ItemTemplate` пользователь щелкнул; и второй, как мы определить `CategoryID` выполнен щелчок LinkButton которого соответствующей категории?

LinkButton как элементы управления кнопок имеет `Click` событий и [ `Command` событие](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Позволяет просто Обратите внимание, что был выполнен щелчок LinkButton событий. Время от времени Однако помимо отметить, что был выполнен щелчок LinkButton мы также необходимо передать некоторыми дополнительными сведениями в обработчик событий. Если это так, LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) и [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) свойств можно назначить эти дополнительные данные. Затем, при нажатии элемента управления LinkButton его `Command` события (вместо его `Click` событий) и обработчик событий передается значения `CommandName` и `CommandArgument` свойства.

При `Command` события из шаблона на повторителя s повторителя [ `ItemCommand` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) срабатывает и передается `CommandName` и `CommandArgument` значения выбранной LinkButton (или кнопки или ImageButton). Таким образом чтобы определить, когда был выполнен щелчок LinkButton в повторителя категории, необходимо сделать следующее:

1. Задать `CommandName` свойство LinkButton в повторителя s `ItemTemplate` с некоторым значением (я открывалось ListProducts). Установив это `CommandName` значение LinkButton s `Command` событие возникает при щелчке элемента управления LinkButton.
2. Набор LinkButton s `CommandArgument` значение текущего элемента s `CategoryID`.
3. Создайте обработчик событий для повторителя s `ItemCommand` событий. В обработчике набор событий `CategoryProductsDataSource` ObjectDataSource s `CategoryID` значение переданного параметра `CommandArgument`.

Следующие `ItemTemplate` разметку для категорий повторителя реализует шаги 1 и 2. Обратите внимание как `CommandArgument` элемент данных s было назначено значение `CategoryID` с использованием синтаксиса привязки данных:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

При каждом создании `ItemCommand` обработчик событий, рекомендуется сначала всегда проверьте входящего `CommandName` поскольку *любой* `Command` событий, вызванных *любой* кнопки, LinkButton, или ImageButton внутри повторителя вызовет `ItemCommand` срабатывание события. Пока у нас в настоящее время имеется только один LinkButton сейчас, в будущем мы (или другой разработчик нашей команды) может добавить дополнительные кнопки веб-элементы управления повторителя, при щелчке вызывает же `ItemCommand` обработчика событий. Таким образом, он s, лучше всего всегда убедитесь, что выбран `CommandName` свойство и только продолжите программный логику, если он соответствует на значение, ожидаемое.

Убедившись, что переданный `CommandName` ListProducts имеет значение, назначает обработчик событий `CategoryProductsDataSource` ObjectDataSource s `CategoryID` значение переданного параметра `CommandArgument`. Такое изменение ObjectDataSource s `SelectParameters` автоматически вызывает DataList повторно привязать себя к источнику данных с продуктами для новой выбранной категории.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

С этими дополнениями наш учебный курс завершается. Теперь пора проверить его в обозревателе. Рис. 14 показан экран, при первом посещении страницы. Поскольку категории еще должен быть выбран, продукты не отображаются. При щелчке на категории, например фруктов, отображаются эти продукты в категории продуктов в виде двух столбцов (см. рис. 15).


[![Нет продуктов, которые отображаются при первом просмотре странице](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Рис. 14**: нет продуктов, которые отображаются при первом просмотре странице ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Щелкнув списки категории создают соответствующие продукты вправо](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Рис. 15**: щелкнув создают категории перечислены продукты сопоставления вправо ([Просмотр полноразмерное изображение](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Сводка

Как мы видели в этом учебнике и предыдущим, главного и подчиненного представлений отчетов могут быть разбросаны по две страницы, или объединены на одном. Отображение сведения об образце отчета на одной странице, однако возникают определенные проблемы, о том, как лучше всего макета главного и сведения о записях на этой странице. В *главный/Detail, с помощью выбираемого основного элемента GridView с DetailsView сведения* учебника мы должны были находиться выше основных записей записи сведения о; в этом учебнике мы использовали методы CSS обязательно float основных записей в слева от сведений.

Вместе с отображение отчетов основной подробности, мы имели возможность узнать, как получить число продукты, связанные с каждой категории, а также как для выполнения логики на стороне сервера, когда LinkButton (или кнопки или ImageButton) нажимается изнутри Повторителя.

Этот учебник завершает изучение главного и подчиненного представлений отчетов с DataList и повторителя. Наш следующий набор учебников показано добавление, изменение и удаление возможностей для элемента управления DataList.

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) учебник по перемещаемые элементы CSS с помощью CSS
- [Позиционирование CSS](http://www.brainjar.com/css/positioning/) Дополнительные сведения на размещение элементов с помощью CSS
- [Размещение Out содержимого с помощью HTML](http://www.w3schools.com/html/html_layout.asp) с помощью `<table>` s и другими элементами HTML для позиционирования

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было Зак Джонс. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [Вперед](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
