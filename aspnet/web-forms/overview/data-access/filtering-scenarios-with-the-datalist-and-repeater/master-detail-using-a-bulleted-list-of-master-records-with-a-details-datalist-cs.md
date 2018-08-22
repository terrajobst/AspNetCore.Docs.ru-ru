---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: «Основной/подробности» с использованием маркированного списка основных записей с элементом управления DataList сведений (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы рассмотрим сжатие две страницы "основной/подробности" отчет о предыдущем учебном курсе на одной странице, которой отображается маркированный список имена категорий на t...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a7c7494a58fa7941924145805f32aa67164fac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829880"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>«Основной/подробности» с использованием маркированного списка основных записей с элементом управления DataList сведений (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) или [скачать PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> В этом руководстве мы рассмотрим сжатие две страницы "основной/подробности" отчет о предыдущем учебном курсе на одной странице, которой отображается маркированный список имена категорий в левой части экрана и в правой части экрана продукты выбранной категории.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](master-detail-filtering-acess-two-pages-datalist-cs.md) мы рассмотрели размещение отчета «основной/подробности» на двух страницах. На главной странице мы использовали элемент управления Repeater для отображения маркированного списка категорий. Имя каждой категории было гиперссылкой, при нажатии бы взять пользователя на страницу сведений о, где двухстолбцовый элемент управления DataList двух столбцов отображались продукты принадлежащих выбранной категории.

В этом руководстве мы рассмотрим сжатие руководства две страницы на одной странице, которой отображается маркированный список имена категорий в левой части экрана названия каждой категории, отображаемой в качестве LinkButton. Щелкнув одну из имя категории элементов управления LinkButton вызывает обратную передачу и привязывает продукты выбранной категории s двухстолбцовый элемент управления DataList, в правой части экрана. Помимо отображения название каждой категории s, элементу управления Repeater в левой части показывает, сколько всего продуктов для данной категории (см. рис. 1).


[![Имя категории и общее число продуктов отображаются в левой части](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Рис. 1**: категории — имя и общее число продуктов отображаются в левой части ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Шаг 1: Отображение элемента Repeater в левой части экрана

В этом руководстве мы должны иметь маркированный список категорий появлялся слева от продуктов выбранной категории s. Содержимого с веб-страниц, может быть настроено с помощью стандартных тегов абзаца элементы HTML, неразрывные пробелы `<table>` s и т. д. или использованием методик, каскадные таблицы стилей (CSS). Всех наших учебниках до сих использовались приемы CSS для позиционирования. При создании пользовательского интерфейса переходов в нас на главной странице в [главные страницы и переходы по узлу](../introduction/master-pages-and-site-navigation-cs.md) руководстве мы использовали *абсолютное позиционирование*, указывающее, точное смещение в пикселях для навигации список и основного содержимого. Кроме того, можно использовать CSS для размещения одного элемента справа или слева от другого с помощью *с плавающей запятой*. У нас есть маркированный список категорий появлялся слева от продуктов выбранной категории s плавающее Repeater слева от элемента управления DataList

Откройте `CategoriesAndProducts.aspx` странице из `DataListRepeaterFiltering` папку и добавьте на страницу элемент управления Repeater и DataList. Значение элемента управления Repeater s `ID` для `Categories` и DataList s для `CategoryProducts`. Перейдите в представление источника и поместить в элементы управления Repeater и DataList `<div>` элементов. То есть, заключите элементу управления Repeater в `<div>` элемент первой и затем DataList в свой собственный `<div>` элемент сразу после элемента управления Repeater. На этом этапе ваш разметка должна выглядеть следующим образом:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Для перемещения элемента управления Repeater слева от DataList, нам нужно использовать `float` атрибут стиля CSS, следующим образом:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;` Помещает первый `<div>` элемент слева от второго. `width` И `padding-right` определяют ширину первого `<div>` s `width` и размер внутреннего отступа между добавляется `<div>` содержимое элемента s и его правым полем. Дополнительные сведения по плавающим элементам в CSS извлечь [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Вместо того чтобы указать параметр стиля непосредственно через первый `<p>` элемент s `style` атрибут, давайте вместо этого создайте новый класс CSS в s `Styles.css` с именем `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Затем можно заменить `<div>` с `<div class="FloatLeft">`.

После добавления класса CSS и настройки разметки в `CategoriesAndProducts.aspx` страницы, перейдите в конструктор. Вы должны увидеть элемент управления Repeater, с плавающей запятой слева от элемента управления DataList (несмотря на то, что право Теперь оба просто отображается как серый полей с момента мы ve еще, чтобы настроить их источники данных или шаблоны).


[![Плавающий элемент управления Repeater слева от элемента управления DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Рис. 2**: плавающий элемент управления Repeater слева от элемента управления DataList ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Шаг 2: Определение количества продуктов для каждой категории

С помощью элемента управления Repeater и DataList s вокруг готова мы будет готов, чтобы привязать данные категории к элементу управления Repeater управлять. Тем не менее как маркированный список категорий на рис. 1 показано, помимо s название каждой категории также необходимо отобразить количество продуктов, связанных с категорией. Для доступа к этой информации мы можете сделать следующее.

- **Получить эту информацию из класс фонового кода s страницы ASP.NET.** Для данного идентификатора *`categoryID`* мы можете определить количество соответствующих продуктов, вызвав `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод. Этот метод возвращает `ProductsDataTable` которого `Count` свойство указывает, сколько `ProductsRow` s существует, то есть числа продуктов для указанного *`categoryID`*. Мы можем создать `ItemDataBound` обработчик событий для элемента управления Repeater, для каждой категории, привязанного к элементу управления Repeater, вызывает `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод и включает счетчик в выходных данных.
- **Обновление `CategoriesDataTable` в типизированном наборе DataSet, включают `NumberOfProducts` столбца.** Затем можно обновлять `GetCategories()` метод в `CategoriesDataTable` для включения этих сведений или оставить `GetCategories()` как- и создать новый `CategoriesDataTable` метод под названием `GetCategoriesAndNumberOfProducts()`.

Позвольте s изучите оба этих метода. Первый подход проще реализовать, так как мы кое t необходимости обновлять уровень доступа к данным; Однако это потребует дополнительных обращений к базе данных. Вызов `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод в `ItemDataBound` обработчик событий добавляет дополнительный вызов базы данных для каждой категории отображаются в элементу управления Repeater. С помощью этого способа есть *N* + 1 вызов базы данных, где *N* число категорий, отображенных в элементу управления Repeater. Во втором подходе число продуктов возвращается с информацией о каждой категории из `CategoriesBLL` класс s `GetCategories()` (или `GetCategoriesAndNumberOfProducts()`) метода, в результате чего лишь одного обращения к базе данных.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Определение количества продуктов в обработчике событий ItemDataBound

Определение количества продуктов для каждой категории в элементу управления Repeater s `ItemDataBound` обработчик событий не требует каких-либо изменений нашей существующего уровня доступа к данным. Все изменения можно внести непосредственно в `CategoriesAndProducts.aspx` страницы. Начните с добавления нового источника ObjectDataSource с именем `CategoriesDataSource` через смарт-тег элемента управления Repeater s. Настройте `CategoriesDataSource` ObjectDataSource, так что он извлекает свои данные из `CategoriesBLL` класс s `GetCategories()` метод.


[![Настройка ObjectDataSource на использование s метода GetCategories() класса CategoriesBLL](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Рис. 3**: Настройка ObjectDataSource для использования `CategoriesBLL` класс s `GetCategories()` метод ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Каждый элемент в `Categories` Repeater должен быть активны и, при нажатии `CategoryProducts` DataList, должны отображаться продукты выбранной категории. Это можно сделать, сделав каждой категории гиперссылки, связывание вернитесь на эту же страницу (`CategoriesAndProducts.aspx`), но передав `CategoryID` через строку запроса, примерно так же, что мы видели в предыдущем учебном курсе. Преимуществом такого подхода — что отображение продуктов определенной категории s страницы могут добавляться в закладки или проиндексирован поисковую систему.

Кроме того мы можем сделать каждой категории LinkButton, именно такой подход, который будет использоваться в этом руководстве. LinkButton готовит к просмотру в браузере пользователя s как гиперссылка, но, при щелчке, вызывает обратную передачу; При обратной передаче DataList s ObjectDataSource необходимо обновить для отображения этих продуктов, принадлежащих выбранной категории. В этом учебнике гиперссылку обретает смысл, чем использование LinkButton; Тем не менее могут существовать другие сценарии, в которых использование LinkButton является более выгодным. Хотя гиперссылки будет идеальным для данного примера, позвольте s вместо просматривать с помощью элемента управления LinkButton. Как мы увидим, с помощью LinkButton возникают определенные проблемы, которые в противном случае не возникает с гиперссылкой. Таким образом с помощью LinkButton в этом учебнике будет выявить эти проблемы и поможет предоставить решения для этих сценариев, где требуется использовать LinkButton вместо гиперссылки.

> [!NOTE]
> Вы являетесь рекомендуется повторить описанные в этом руководстве, с помощью элемента управления HyperLink или `<a>` элемента вместо элемента управления LinkButton.


В следующей разметке показан декларативный синтаксис для элемента управления Repeater и элемент управления ObjectDataSource. Обратите внимание, что элемент управления Repeater s отображают маркированный список с каждым элементом как LinkButton.


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> В этом руководстве Repeater должен иметь состояние представления включено (Обратите внимание, из-за отсутствия `EnableViewState="False"` из декларативного синтаксиса элемента управления Repeater s). На шаге 3 мы создадим обработчик событий для элемента управления Repeater s `ItemCommand` событий, в котором мы будем обновлять DataList s ObjectDataSource s `SelectParameters` коллекции. Элемент управления Repeater s `ItemCommand`, но при этом выиграл t fire, если состояние представления отключено. См. в разделе [головоломка возникает вопрос ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) и [свое решение](http://scottonwriting.net/sowBlog/posts/1268.aspx) Дополнительные сведения о том, почему состояние представления должно быть включено для элемента управления Repeater s `ItemCommand` событие.


ImageButton с `ID` значение свойства `ViewCategory` не поддерживает его `Text` набор свойств. Если мы хотели только отображаемое имя категории, мы бы свойство Text декларативно, через синтаксис привязки данных, следующим образом:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Тем не менее, мы хотим Показать имя категории s *и* количество продуктов, принадлежащих к этой категории. Эти сведения можно получить из элемента управления Repeater s `ItemDataBound` обработчик событий с помощью вызова `ProductBLL` класс s `GetCategoriesByProductID(categoryID)` и определения, сколько записей возвращаются в результирующем `ProductsDataTable`, как приведенный ниже иллюстрирует:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Мы начинаем с гарантий, что мы повторно работа с элементом данных (один, `ItemType` — `Item` или `AlternatingItem`), а затем укажите `CategoriesRow` экземпляр, который только что был привязан к текущему `RepeaterItem`. Далее определяется количество продуктов для данной категории, создав экземпляр `ProductsBLL` , вызова его `GetCategoriesByProductID(categoryID)` метод и определения количества записей, возвращаемых с помощью `Count` свойство. Наконец `ViewCategory` LinkButton в ItemTemplate — references и его `Text` свойству *CategoryName* (*NumberOfProductsInCategory*), где  *NumberOfProductsInCategory* форматируется как число без дробных разрядов.

> [!NOTE]
> Кроме того, можно было добавить *функцию форматирования* в класс фонового кода страницы s ASP.NET, получающую s `CategoryName` и `CategoryID` значения и возвращает `CategoryName` объединяется с числом продукты в категории (как определено вызовом `GetCategoriesByProductID(categoryID)` метод). Результаты функции форматирования можно декларативно присвоить s LinkButton свойство Text, нет необходимости в `ItemDataBound` обработчик событий. Ссылаться на [использование полей TemplateField в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) или [форматирование элементов управления DataList и Repeater от данных](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) учебники, Дополнительные сведения об использовании функций форматирования.


После добавления этого обработчика событий, Отвлекитесь и проверьте страницу в обозревателе. Обратите внимание на то, как каждая категория отображается в виде маркированного списка, отображая имя категории s и количество продуктов, связанных с категорией (см. рис. 4).


[![Отображаются каждой категории — имя и число продуктов](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Рис. 4**: отображается каждая категория — имя и число продуктов ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Обновление`CategoriesDataTable`и`CategoriesTableAdapter`для включения количества продуктов для каждой категории

Вместо определения количества продуктов для каждой категории, так как он с привязкой к элементу управления Repeater, этот процесс можно ускорить с помощью `CategoriesDataTable` и `CategoriesTableAdapter` уровне доступа к данным для включения этих сведений. Для этого необходимо добавить новый столбец для `CategoriesDataTable` для хранения количества соответствующих продуктов. Чтобы добавить новый столбец в таблицу данных, откройте типизированный набор DataSet (`App_Code\DAL\Northwind.xsd`), щелкните правой кнопкой мыши на объект DataTable для изменения и выберите Add / столбца. Добавить новый столбец для `CategoriesDataTable` (см. рис. 5).


[![Добавление нового столбца к CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Рис. 5**: добавьте новый столбец для `CategoriesDataSource` ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Это будет добавлен новый столбец с именем `Column1`, которые можно изменить, просто введя другое имя. Переименуйте этот новый столбец в `NumberOfProducts`. Далее нам нужно настроить этот столбец s свойства. Щелкните новый столбец и перейдите в окно свойств. Изменить столбец s `DataType` свойства из `System.String` для `System.Int32` и задайте `ReadOnly` свойства `True`, как показано на рис. 6.


![Устанавливать свойства только для чтения нового столбца и тип данных](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Рис. 6**: задать `DataType` и `ReadOnly` свойства нового столбца


Хотя `CategoriesDataTable` теперь имеет `NumberOfProducts` столбец, его значение не устанавливается в любой из соответствующих s запросов адаптера таблицы. Корпорация Майкрософт может обновлять `GetCategories()` метод для возврата этих сведений, если требуется такая информация возвращается каждый раз, чтобы получить сведения о категории. Если, однако нам требуется только получение количества соответствующих продуктов для категорий в редких случаях (например, как только для этого руководства), то можно оставить `GetCategories()` как- и создать новый метод, возвращающий данную информацию. S позволяют использовать такой подход адекватен, создадим новый метод с именем `GetCategoriesAndNumberOfProducts()`.

Чтобы добавить этот новый `GetCategoriesAndNumberOfProducts()` метод, щелкните правой кнопкой мыши `CategoriesTableAdapter` и выберите новый запрос. Это дает вверх TableAdapter запроса мастер настройки, которые мы ve, используемый в предыдущих учебных курсах. Этот метод запустите мастер, указав, что запрос использует специальный оператор SQL, возвращающий строки.


[![Создание метода, использующего специальный оператор SQL](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Рис. 7**: создайте метод с помощью инструкции SQL Ad-Hoc ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![Строки, возвращенные инструкцией SQL](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Рис. 8**: Rows возвращает инструкция SQL ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


На следующем экране мастера запрос на указание запрос для использования. Для возвращения каждой категории s `CategoryID`, `CategoryName`, и `Description` поля, а также количество продуктов, связанных с категорией, используйте следующую команду `SELECT` инструкции:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Укажите запрос для использования](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Рис. 9**: укажите запрос для использования ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Обратите внимание, что, вычисляющего количество продуктов, связанных с категорией псевдонимом `NumberOfProducts`. Это соответствие имен вызывает связывание значения, возвращенного этот вложенный запрос, которым будет связана `CategoriesDataTable` s `NumberOfProducts` столбца.

После ввода этого запроса, последний шаг — это выбрать имя для нового метода. Используйте `FillWithNumberOfProducts` и `GetCategoriesAndNumberOfProducts` для заполнения таблицы данных и возвращают объект DataTable шаблоны, соответственно.


[![Имя нового имена FillWithNumberOfProducts методы TableAdapter s и GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Рис. 10**: имя нового адаптера таблицы s методы `FillWithNumberOfProducts` и `GetCategoriesAndNumberOfProducts` ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


На этом этапе уровня доступа к данным был расширен для включения количества продуктов по категориям. Так как все слой презентации направляет все вызовы DAL через отдельный уровня бизнес-логики необходимо добавить соответствующий `GetCategoriesAndNumberOfProducts` метод `CategoriesBLL` класса:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

С помощью DAL и BLL, мы повторно готовы привязать эти данные `Categories` Repeater в `CategoriesAndProducts.aspx`! Если ve вы уже создали элемента ObjectDataSource для элемента управления Repeater от Определение числа продуктов в `ItemDataBound` раздела с обработчиком событий, удалить данный элемент управления ObjectDataSource и элементу управления Repeater s `DataSourceID` свойство; также принадлежащего Элемент управления Repeater s `ItemDataBound` событие из обработчика событий, удаляя `Handles Categories.OnItemDataBound` синтаксис в классе фонового кода ASP.NET.

С помощью элемента управления Repeater, обратно в исходное состояние, добавить новый ObjectDataSource, именуемый `CategoriesDataSource` через смарт-тег элемента управления Repeater s. Настройка ObjectDataSource на использование `CategoriesBLL` класс, но вместо использования `GetCategories()` метод, использование `GetCategoriesAndNumberOfProducts()` вместо (см. рис. 11).


[![Настройка ObjectDataSource для использования метода GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Рис. 11**: Настройка ObjectDataSource для использования `GetCategoriesAndNumberOfProducts` метод ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Затем обновите `ItemTemplate` таким образом, LinkButton s `Text` свойство назначается декларативно с помощью синтаксиса привязки данных и включает в себя `CategoryName` и `NumberOfProducts` полей данных. Полная декларативная разметка для элемента управления Repeater и `CategoriesDataSource` ObjectDataSource выполните:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

Выходные данные, выводимый путем обновления DAL для включения `NumberOfProducts` столбец является так же, как `ItemDataBound` подход обработчика событий (см. рис. 4 Чтобы увидеть снимок элементу управления Repeater, в котором отображаются имена категорий и количество продуктов).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Шаг 3: Отображение продуктов выбранной категории s

На этом этапе у нас есть `Categories` Repeater, отображение списка категорий, а также количество продуктов в каждой категории. Элементу управления Repeater используется LinkButton для каждой категории, что, при щелчке, вызывает обратную передачу, по которому указывают мы должны отображаться продукты выбранной категории в `CategoryProducts` DataList.

Трудность, с которыми мы, как у элемента управления DataList отображаться только продукты выбранной категории. В [Master/Detail, с помощью выбираемого основного элемента GridView с помощью DetailsView сведения](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) руководства мы узнали, как для построения элемента управления GridView, строки которого может быть выбрана, с выбранной строки s сведений, отображаемых в элементе управления DetailsView на этой же странице. GridView s ObjectDataSource возвратил информацию обо всех продуктах с помощью `ProductsBLL` s `GetProducts()` метода во время преобразования DetailsView ObjectDataSource получил информацию о выбранном продукте с помощью `GetProductsByProductID(productID)` метод. *`productID`* Было предоставлено декларативно путем его сопоставления со значение GridView s `SelectedValue` свойство. К сожалению, нет Repeater `SelectedValue` свойство и не может служить в качестве источника параметра.

> [!NOTE]
> Это одна из проблем, возникающих при использовании элемента управления LinkButton в элементе управления Repeater. Мы использовали гиперссылки для передачи в `CategoryID` через строку запроса вместо этого мы использовать это поле строки запроса как источник для значения параметра s.


Прежде чем думать об отсутствии `SelectedValue` свойства для элемента управления Repeater, однако let s сначала привязать элемент управления DataList к элементу ObjectDataSource и указать его `ItemTemplate`.

В смарт-теге элемента управления DataList s, необязательно, чтобы добавить новый ObjectDataSource, именуемый `CategoryProductsDataSource` и настройте его для использования `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод. Так как элемент управления DataList, в этом учебнике предоставляет интерфейс только для чтения, вы можете задать раскрывающиеся списки в INSERT, UPDATE и удаление вкладок (нет).


[![Настройка ObjectDataSource на использование метод GetProductsByCategoryID(categoryID) класса ProductsBLL s](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Рис. 12**: Настройка ObjectDataSource для использования `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Так как `GetProductsByCategoryID(categoryID)` метод ожидает входной параметр (*`categoryID`*), мастер настройки источников данных можно указать источник параметра s. Категории были перечислены в элементе управления GridView или DataList, мы d значение параметра исходного стрелку раскрывающегося списка элемента управления, а для ControlID – `ID` данных веб-элемента управления. Однако, поскольку отсутствует элемент управления Repeater `SelectedValue` свойство, он не может использоваться в качестве источника параметра. Если выбран, вы обнаружите, что раскрывающемся списке ControlID содержит только один элемент управления `ID``CategoryProducts`, `ID` элемента управления DataList.

Пока значение параметра исходного стрелку раскрывающегося списка None. Мы получим программного связывания значение этого параметра, при нажатии элемента управления LinkButton нажатии в элементу управления Repeater.


[![Сделать не указан источник для параметра categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Рис. 13**: не указан источник для *`categoryID`* параметра ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


После завершения работы мастера настройки источников данных Visual Studio автоматически создает элемент управления DataList s `ItemTemplate`. Замените это значение по умолчанию `ItemTemplate` с шаблоном мы используется в предыдущем учебном курсе; Кроме того, набор элементов управления DataList s `RepeatColumns` значение 2. После внесения этих изменений декларативная разметка для DataList и его связанный элемент управления ObjectDataSource должен выглядеть следующим образом:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

В настоящее время `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* не устанавливалось, поэтому продукты не отображаются при просмотре страницы. Нам нужно будет иметь значение этого параметра устанавливалось в соответствии `CategoryID` выбранной категории в элементу управления Repeater. Это создает две проблемы: во-первых, как мы определяем при LinkButton в элементу управления Repeater s `ItemTemplate` щелкнули; и во-вторых, как определить `CategoryID` соответствующей категории была нажата, LinkButton?

У элемента управления LinkButton, как элементы управления кнопок `Click` событий и [ `Command` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Событий позволяет просто Обратите внимание, что был выполнен щелчок LinkButton. В некоторых случаях Однако помимо отметить, что был выполнен щелчок LinkButton также необходимо передать обработчику события определенную дополнительную информацию. Если это так, LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) и [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) может быть присвоена дополнительную информацию. Затем, при щелчке элемента управления LinkButton, его `Command` вызывает событие (а не его `Click` событий) и обработчику события передаются значения `CommandName` и `CommandArgument` свойства.

При `Command` было порождено в шаблоне элемента Repeater, Repeater s [ `ItemCommand` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) и ему передаются `CommandName` и `CommandArgument` значения выбранной LinkButton (или кнопки или ImageButton). Таким образом чтобы определить, когда был выполнен щелчок LinkButton в элементу управления Repeater категории, необходимо сделать следующее:

1. Задайте `CommandName` свойство элемента управления LinkButton в элементу управления Repeater s `ItemTemplate` некоторое значение (я открывалось ListProducts). Задав для него `CommandName` значение LinkButton s `Command` событие запускается при нажатии элемента управления LinkButton.
2. Набор LinkButton s `CommandArgument` значение текущего элемента s `CategoryID`.
3. Создайте обработчик событий для элемента управления Repeater s `ItemCommand` событий. В обработчике набор событий `CategoryProductsDataSource` ObjectDataSource s `CategoryID` значение переданного параметра `CommandArgument`.

Следующие `ItemTemplate` разметки для элемента управления Repeater категории реализует шаги 1 и 2. Обратите внимание как `CommandArgument` значения элемента данных s `CategoryID` используя синтаксис привязки данных:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

При каждом создании `ItemCommand` обработчик событий, рекомендуется всегда сначала проверять входящее значение `CommandName` поскольку значение *любой* `Command` вызывает событие *любой* кнопка, LinkButton, или Вызовет ImageButton в элементе Repeater `ItemCommand` событие. Хотя в настоящее время только теперь у нас один LinkButton, в будущем мы (или другие разработчики нашей команды) может добавить дополнительную кнопку веб-элементов управления к элементу управления Repeater, при нажатии вызывает же `ItemCommand` обработчик событий. Таким образом, он s, следует всегда убедитесь, что выбран `CommandName` свойство и только приступить программной логикой, если он соответствует значение, ожидаемое.

Убедившись, что переданный `CommandName` значение равно ListProducts, обработчик событий, после чего присваивает ей `CategoryProductsDataSource` ObjectDataSource s `CategoryID` значение переданного параметра `CommandArgument`. Такое изменение принадлежащего элементу ObjectDataSource s `SelectParameters` автоматически вынуждает элемент управления DataList, чтобы заново привязать себя к источнику данных, с продуктами для новой выбранной категории.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

На этих дополнениях наш учебный курс завершается. Отвлекитесь и протестировать его в браузере. Рис. 14 показано окно, при первом просмотре странице. Поскольку категория еще необходимо выбрать, продукты не отображаются. При щелчке на категории, например Produce, отображаются продукты категории продукта в виде двух столбцов (см. рис. 15).


[![Продукты не будут отображаться при первом просмотре страницы](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Рис. 14**: нет продуктов, которые отображаются при первом просмотре страницы ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Щелкнув отображаются категории Produce соответствующие продукты справа](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Рис. 15**: щелкнув этой категории будут перечислены продукты сопоставления справа ([Просмотр полноразмерного изображения](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Сводка

Как мы видели в этом руководстве и предыдущим, отчеты «основной/подробности» могут быть распределены по всему на двух страницах или консолидироваться на одной. Отображение отчета основной/подробности на одной странице, тем не менее, возникают определенные проблемы о том, как лучше всего для главного макета и сведения о записях на этой странице. В *Master/Detail, с помощью выбираемого основного элемента GridView с помощью DetailsView сведения* руководства, у нас было выше основных записей отображаются записи сведения; в этом руководстве мы использовали методы CSS для float основных записей в Левая граница элемента сведений.

Помимо отображения обобщенных и подробных отчетов, мы имели возможность исследуется способ извлечения нескольких продуктов, связанных с каждой категорией также, как для выполнения логики на стороне сервера, когда LinkButton (или кнопки или ImageButton) нажатии изнутри Элемент управления Repeater.

Этот учебник завершает изучение отчеты «основной/подробности» с помощью элементов управления DataList и Repeater. Наш следующий набор учебных курсов будет посвящен добавлению редактирования и удаления возможности для элемента управления DataList.

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) учебник по плавающим элементам в CSS
- [Положение в CSS](http://www.brainjar.com/css/positioning/) Дополнительные сведения о размещении элементов с CSS
- [Размещение Out содержимого с HTML](http://www.w3schools.com/html/html_layout.asp) с помощью `<table>` s и другие элементы HTML для размещения

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был Джонс (Zack Jones). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [Вперед](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
