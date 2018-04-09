---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Декларативные параметры (Visual Basic) | Документы Microsoft
author: rick-anderson
description: В этом учебнике показано использование параметров с жестко запрограммированных значений для выбора данных для отображения в элементе управления DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 933b7276c6dac5cce0e278fd23ff010c5b4a6fdd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="declarative-parameters-vb"></a>Декларативные параметры (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) или [скачать PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> В этом учебнике показано использование параметров с жестко запрограммированных значений для выбора данных для отображения в элементе управления DetailsView.


## <a name="introduction"></a>Вступление

В [последнего учебника](displaying-data-with-the-objectdatasource-vb.md) мы рассматривали отображение данных с помощью элементов управления GridView, DetailsView и FormView, привязанных к элементу управления ObjectDataSource, вызвавшей `GetProducts()` метод `ProductsBLL` класса. `GetProducts()` Метод возвращает строго типизированный объект DataTable, заполненный все записи из базы данных Northwind `Products` таблицы. `ProductsBLL` Содержит дополнительные методы для возврата отдельных наборов продуктов - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, и `GetProductsBySupplierID(supplierID)`. Эти три метода ожидают, что входной параметр, указывающий на способ фильтрации возвращенной информации о продукте.

ObjectDataSource может использоваться для вызова методов, которым необходимы входные параметры, но для этого необходимо указать, откуда берутся значения для этих параметров. Значения параметров могут быть жестко или могут поступать из различных динамических источников, включая: значения строки запроса, переменные сеанса значение свойства веб-элемента управления на странице или другим пользователям.

В этом учебнике начнем с демонстрации использования параметров с жестко запрограммированных значений. В частности, мы рассмотрим добавление DetailsView страницу, которая отображает сведения об определенном продукте, а именно Креольская Gumbo набор, который имеет `ProductID` 5. Далее будет показано, как задать значение параметра, основанное на веб-элемента управления. В частности мы будем использовать текстовое поле, в котором пользователь может ввести название страны, после чего необходимо щелкнуть кнопку, чтобы просмотреть список поставщиков, которые находятся в данной стране.

## <a name="using-a-hard-coded-parameter-value"></a>С помощью значения параметра жестко запрограммированный

В первом примере, начните с добавления элемента управления DetailsView для `DeclarativeParams.aspx` страницы в `BasicReporting` папки. DetailsView смарт-тега, выберите &lt;новый источник данных&gt; из раскрывающегося списка и выберите команду Добавить элемент управления ObjectDataSource.


[![Добавить на страницу элемент управления ObjectDataSource](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Рис. 1**: добавить на страницу элемент управления ObjectDataSource ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image3.png))


Автоматически запустится мастер управления ObjectDataSource Выбор источника данных. Выберите `ProductsBLL` класса на первом экране мастера.


[![Выберите класс ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**На рисунке 2**: выберите `ProductsBLL` класса ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image6.png))


Поскольку мы хотим просмотреть сведения об определенном продукте, мы хотим использовать `GetProductByProductID(productID)` метода.


[![Выберите метод GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Рис. 3**: выберите `GetProductByProductID(productID)` метод ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image9.png))


Поскольку мы выбрали метода включает параметр, есть еще один экран мастера с запросом для определения значения для параметра. В списке в левой части показаны все параметры для выбранного метода. Для `GetProductByProductID(productID)` имеется только один `productID`. В правой части можно указать значение для выбранного параметра. Список раскрывающегося списка параметров источника перечисляет различные возможные источники для значения параметра. Поскольку мы хотим указать жестко запрограммированных значений 5 для `productID` источника параметра ни один параметр, и введите в текстовое поле DefaultValue 5.


[![Hard-Coded параметр из 5 будет использоваться значение для параметра код продукта](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Рис. 4**: A Hard-Coded параметр значение 5 будет использоваться для `productID` параметра ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image12.png))


После завершения работы мастера настройки источника данных включает декларативная разметка элемента управления ObjectDataSource `Parameter` объекта в `SelectParameters` коллекции для каждого входного параметра с помощью метода, определенного в `SelectMethod` свойство. Так как мы используем в этом примере метод ожидает только один входной параметр, `parameterID`, имеется только одна запись здесь. `SelectParameters` Коллекция может содержать любой класс, производный от `Parameter` класса в `System.Web.UI.WebControls` пространства имен. Для базового значения параметров, жестко `Parameter` класс используется, но для параметра другие параметры производный источника `Parameter` используется класс; можно также создать свои собственные [пользовательские типы параметров](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), при необходимости.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Если вы разрабатываете головоломку на своем компьютере декларативная разметка отображается на этом этапе может включать значения для `InsertMethod`, `UpdateMethod`, и `DeleteMethod` свойства, а также `DeleteParameters`. ObjectDataSource Выбор источника данных мастер автоматически определяет методы из `ProductBLL` для использования вставки, обновления и удаления, поэтому если вы явно не удалить, они будут включены в указанную выше разметку.


При посещении этой страницы данных веб-элемент управления будет вызывать ObjectDataSource `Select` метод, который будет вызывать `ProductsBLL` класса `GetProductByProductID(productID)` метод, использование жестко запрограммированных значений 5 для `productID` входного параметра. Возвращает строго типизированный метод `ProductDataTable` объект, который содержит единственную строку с информацией о Креольская Gumbo Mix (продукт с `ProductID` 5).


[![Отображаются сведения о Креольская огненная смесь](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Рис. 5**: отображаются сведения о Креольская Gumbo Mix ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Задание значения параметра к значению свойства веб-элемента управления

Также можно задать значения параметра ObjectDataSource на основе значения веб-элемента управления на странице. Чтобы проиллюстрировать это, рассмотрим элемент GridView, со списком всех поставщиков, расположенных в стране, заданное пользователем. Для этого, добавив текстовое поле на страницу, в который пользователь может ввести название страны. Задать для элемента управления TextBox `ID` свойства `CountryName`. Также можно добавьте кнопку веб-элемент управления.


[![Текстовое поле на страницу с Идентификатором CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Рис. 6**: текстовое поле на страницу с `ID` `CountryName` ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image18.png))


Затем добавьте элемент управления GridView на страницу и смарт-теге выберите Добавить новый элемент управления ObjectDataSource. Поскольку нам требуется отобразить сведения выберите поставщика `SuppliersBLL` класс из первое окно мастера. Второй экран выберите `GetSuppliersByCountry(country)` метод.


[![Выберите метод GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Рис. 7**: выберите `GetSuppliersByCountry(country)` метод ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image21.png))


Поскольку `GetSuppliersByCountry(country)` метод имеет входной параметр, мастер снова включает последний экран, на выбор значения параметра. На этот раз Задайте источник параметров управления. Это будут заполнять список ControlID раскрывающийся список с именами элементов управления на странице; Выберите `CountryName` управления из списка. При первом посещении страницы `CountryName` текстовое поле будет пустым, поэтому результаты не возвращаются и ничего не отображается. Если вы хотите отображать некоторые результаты по умолчанию, задайте текстовое поле DefaultValue соответствующим образом.


[![Значение параметра присвоено значение элемента управления CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Рис. 8**: задайте значение параметра `CountryName` значение элемента управления ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image24.png))


Декларативная разметка ObjectDataSource немного отличается от первого примера, с помощью [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) вместо стандартной `Parameter` объекта. Объект `ControlParameter` имеет дополнительные свойства для указания `ID` веб-элемента управления и значение свойства, используемое для параметра (`PropertyName`). Мастер настройки источника данных было smart определить, что для текстового поля, мы скорее всего, решите использовать `Text` свойств для значения параметра. Если, однако вы хотите использовать другое значение свойства из веб-элемента управления можно изменить `PropertyName` значение здесь или щелкнув ссылку «Показать дополнительные параметры» в мастере.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

При просмотре страницы в первый раз `CountryName` пуст. Элемент управления ObjectDataSource `Select` по-прежнему вызывается метод GridView, но значение `Nothing` передается в `GetSuppliersByCountry(country)` метод. Преобразует TableAdapter `Nothing` в базу данных `NULL` значение (`DBNull.Value`), но запрос, используемый `GetSuppliersByCountry(country)` написан таким образом, что он не возвращает все значения, когда `NULL` заданозначение`@CategoryID`параметра. Иными словами поставщики не возвращаются.

После посетителя тем не менее, вводит название страны и нажимает кнопку Показать поставщики для вызывает обратную передачу, ObjectDataSource `Select` опросить метод передачи в элементе управления TextBox `Text` как `country` параметр.


[![Отображаются поставщики из Канады](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Рис. 9**: отображаются те поставщики из Канады ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Отображение всех поставщиков по умолчанию

А чем не поставщиков Показывать при первом посещении страницы нужно показать *все* поставщиков, позволяя пользователю сократить список, введя название страны в текстовом поле. Если текстовое поле будет пустым, `SuppliersBLL` класса `GetSuppliersByCountry(country)` методу передается в `Nothing` для его *`country`* входного параметра. Это `Nothing` значение затем передается методу `GetSupplierByCountry(country)` метод, где оно преобразуется в базе данных `NULL` значение для `@Country` параметр в следующем запросе:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Выражение `Country = NULL` всегда возвращает значение False, даже для записей, `Country` столбец имеет `NULL` значением; поэтому записи не возвращаются.

Для возврата *все* поставщиков при пустом поле TextBox пуст, можно расширить `GetSuppliersByCountry(country)` МЕТОДА для вызова метода `GetSuppliers()` метод при значении параметра страны `Nothing` и для вызова МЕТОДА `GetSuppliersByCountry(country)` в противном случае метод. Это окажет влияние возвращение всех поставщиков, если страна не указана и соответствующий набор поставщиков, если включен параметр страны.

Изменение `GetSuppliersByCountry(country)` метод `SuppliersBLL` следующим образом:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Благодаря этому изменению `DeclarativeParams.aspx` страница отобразит все поставщики при первом посещении (или каждый раз, когда `CountryName` пуст).


[![Все поставщики, теперь отображаются по умолчанию](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Рис. 10**: все поставщики, теперь отображаются по умолчанию ([Просмотр полноразмерное изображение](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Сводка

Чтобы использовать методы с входными параметрами, необходимо указать значения для параметров в коллекции `SelectParameters` коллекции. Различные типы параметров допускают значение параметра может быть получена из различных источников. Тип параметра по умолчанию использует жестко запрограммированных значений, но так же, как легко (и без строки кода) значений параметров можно получить из строки запроса, переменные сеанса, файлы cookie и даже введенные пользователем значения из веб-элементов управления на странице.

Примеры, которые мы рассматривали в этом учебнике показано, как использовать декларативные параметра значения. Однако иногда раз, когда необходимо использовать параметр источник, недоступно, например текущей датой и временем, либо, если веб-узле используется членство, идентификатор пользователя посетителя. Для таких сценариев можно задать значения параметров программным путем до ObjectDataSource вызов метода базового объекта. Мы рассмотрим пример приведен в [следующее руководство](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](displaying-data-with-the-objectdatasource-vb.md)
> [Вперед](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
