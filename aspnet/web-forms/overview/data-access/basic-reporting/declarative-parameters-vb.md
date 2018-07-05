---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Декларативные параметры (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве демонстрируют способы использования параметров с жестко запрограммированных значений, чтобы выбрать данные, отображаемые в элементе управления DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 86590faf30b41acdd8a0922f9cc33feabfded23f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397734"
---
<a name="declarative-parameters-vb"></a>Декларативные параметры (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) или [скачать PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> В этом руководстве демонстрируют способы использования параметров с жестко запрограммированных значений, чтобы выбрать данные, отображаемые в элементе управления DetailsView.


## <a name="introduction"></a>Вступление

В [последнего курса](displaying-data-with-the-objectdatasource-vb.md) было рассмаотрено отображение данных с помощью элементов управления GridView, DetailsView и FormView, привязанных к элементу управления ObjectDataSource, вызвавшей `GetProducts()` метода из `ProductsBLL` класса. `GetProducts()` Метод возвращает строго типизированный объект DataTable, заполненный всеми записи из базы данных Northwind `Products` таблицы. `ProductsBLL` Содержит дополнительные методы для возврата отдельных наборов продуктов - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, и `GetProductsBySupplierID(supplierID)`. Этих трех методов необходим входной параметр, указывающий на способ фильтрации возвращенной информации о продукте.

ObjectDataSource может использоваться для вызова методов, которые необходимы входные параметры, но для этого, мы должны указать, откуда берутся значения для этих параметров. Значения параметров могут быть жестко запрограммированы или могут поступать из различных динамических источников, включая: значения строки запроса, переменные сеансов, значение свойства веб-элемента управления на странице или другим пользователям.

В этом руководстве давайте начнем с демонстрации использования параметров с жестко запрограммированных значений. В частности, мы рассмотрим добавление DetailsView на страницу, на которой отображаются сведения об определенном продукте, а именно Chef Anton's Gumbo Mix, для `ProductID` 5. Далее будет показано, как задать значение параметра, основанное на веб-элемент управления. В частности мы будем использовать текстовое поле, чтобы пользователь мог ввести название страны, после чего щелкнуть кнопку и просмотреть список поставщиков, которые находятся в этой стране.

## <a name="using-a-hard-coded-parameter-value"></a>С помощью значения параметра жестко

Для первого примера начнем с добавления элемента управления DetailsView на `DeclarativeParams.aspx` странице в `BasicReporting` папку. Смарт-теге DetailsView выберите &lt;новый источник данных&gt; из раскрывающегося списка и выберите Добавление нового ObjectDataSource.


[![Добавление ObjectDataSource на страницу](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Рис. 1**: Добавление ObjectDataSource на страницу ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image3.png))


Автоматически запустится мастер Выбор источника данных элемента управления ObjectDataSource. Выберите `ProductsBLL` класс на первом экране мастера.


[![Выбор класса](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Рис. 2**: выберите `ProductsBLL` класс ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image6.png))


Поскольку мы хотим отобразить сведения об определенном продукте, мы хотим использовать `GetProductByProductID(productID)` метод.


[![Выберите метод GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Рис. 3**: выберите `GetProductByProductID(productID)` метод ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image9.png))


Поскольку выбранный метод включает параметр, имеется один экран мастера, где вам будет предложено определить значение, используемое для параметра. В списке слева отображаются все параметры для выбранного метода. Для `GetProductByProductID(productID)` имеется только один `productID`. В правой части можно указать значение для выбранного параметра. Список раскрывающегося списка параметров источника перечисляет различные возможные источники для значения параметра. Поскольку нам требуется указать значение 5 для жестко `productID` параметр, оставьте источника параметра выберите None и введите 5 в текстовое поле DefaultValue.


[![Hard-Coded параметр из 5 будет использоваться значение для параметра productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Рис. 4**: A Hard-Coded параметра значение 5 будет использоваться для `productID` параметра ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image12.png))


После завершения работы мастера настройки источника данных включает декларативная разметка элемента управления ObjectDataSource `Parameter` объекта в `SelectParameters` для каждого входного параметра с помощью метода, определенного в `SelectMethod` свойство. Так как он используется в этом примере может принимать только один входной параметр, `parameterID`, имеется только одна запись здесь. `SelectParameters` Коллекция может содержать любой класс, производный от `Parameter` в класс `System.Web.UI.WebControls` пространства имен. Для базового значения параметров, жестко `Parameter` класс используется, но для другого источника параметры производный `Parameter` класс используется; можно также создать свои собственные [пользовательские типы параметров](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), при необходимости.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Если вы разрабатываете головоломку на своем компьютере декларативной разметке появится на этом этапе может включать значения для `InsertMethod`, `UpdateMethod`, и `DeleteMethod` свойства, а также `DeleteParameters`. Выбор источника данных мастер ObjectDataSource автоматически указывает методы `ProductBLL` для использования вставки, обновления и удаления, поэтому если вы явно не удалить, они будут включены в указанную выше разметку.


При посещении этой страницы данных веб-элемента управления вызовет ObjectDataSource `Select` метод, который будет вызывать `ProductsBLL` класса `GetProductByProductID(productID)` метод, используя жестко запрограммированное значение 5 для `productID` входного параметра. Метод возвратит строго типизированный `ProductDataTable` объект, который содержит единственную строку с информацией о Chef Anton's Gumbo Mix (продукт с `ProductID` 5).


[![Отображаются сведения о Chef Anton's Gumbo Mix](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Рис. 5**: отображаются сведения о Chef Anton's Gumbo Mix ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Значение параметра на значение свойства веб-элемента управления

Также можно задать значения параметра ObjectDataSource на основе значения веб-элемента управления на странице. Чтобы проиллюстрировать это, рассмотрим элемент GridView, со списком всех поставщиков, расположенных в стране, указанные пользователем. Для этого, добавив элемент управления TextBox на страницу, в который пользователь может ввести название страны. Установить этот элемент управления TextBox `ID` свойства `CountryName`. Также можно добавьте кнопку веб-элемент управления.


[![Добавление текстового поля на страницу с Идентификатором CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Рис. 6**: Добавление текстового поля на страницу с `ID` `CountryName` ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image18.png))


Затем добавьте элемент управления GridView на страницу и в смарт-теге, выберите для добавления нового источника ObjectDataSource. Поскольку нам требуется отобразить сведения выберите поставщика `SuppliersBLL` класс из первый экран мастера. На втором экране выберите `GetSuppliersByCountry(country)` метод.


[![Выберите метод GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Рис. 7**: выберите `GetSuppliersByCountry(country)` метод ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image21.png))


Так как `GetSuppliersByCountry(country)` метод имеет входной параметр, мастер снова включает последний экран для выбора значения параметра. На этот раз источника параметра установите для элемента управления. Это будут заполнять список ControlID раскрывающегося списка с именами элементов управления на странице; Выберите `CountryName` управления из списка. При первом посещении страницы `CountryName` элемент управления TextBox будет пустым, поэтому результаты не возвращаются и ничего не отображается. Если вы хотите отобразить некоторые результаты по умолчанию, задайте соответствующим образом текстовое поле DefaultValue.


[![Значение параметра равно значение элемента управления CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Рис. 8**: задайте значение параметра `CountryName` значение элемента управления ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image24.png))


Декларативная разметка ObjectDataSource немного отличается от первого примера с помощью [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) вместо стандартного `Parameter` объекта. Объект `ControlParameter` имеет дополнительные свойства для указания `ID` веб-элемента управления и значение свойства, используемое для параметра (`PropertyName`). Мастер настройки источника данных понимал определить, что, для текстового поля, будет скорее всего будет использоваться `Text` свойство для значения параметра. Если, однако вы хотите использовать другое значение свойства из веб-элемента управления можно изменить `PropertyName` здесь или щелкнув ссылку «Показать дополнительные свойства» в мастере.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

При просмотре страницы в первый раз `CountryName` пуст. ObjectDataSource `Select` метод по-прежнему вызывается GridView, но значение `Nothing` передается в `GetSuppliersByCountry(country)` метод. TableAdapter преобразует `Nothing` в базу данных `NULL` значение (`DBNull.Value`), но запрос, используемый методом `GetSuppliersByCountry(country)` написан таким образом, чтобы он не возвращает какие-либо значения, когда `NULL` значение, заданное для `@CategoryID`параметра. Короче говоря поставщики не возвращаются.

Как только посетитель вводит название страны, тем не менее и нажимает кнопку Показать поставщиков для инициирования обратной передачи, элемент управления ObjectDataSource `Select` ObjectDataSource, передавая в элементе управления TextBox `Text` значение как `country` параметр.


[![Показаны поставщики из Канады](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Рис. 9**: показаны поставщики из Канады ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Отображение всех поставщиков по умолчанию

А чем отобразить ни один из поставщиков, при первом посещении страницы нужно показать *все* поставщиков, позволяя пользователю сократить список, введя название страны в текстовом поле. Если поле TextBox пустое, `SuppliersBLL` класса `GetSuppliersByCountry(country)` методу передается в `Nothing` для его *`country`* входного параметра. Это `Nothing` значение затем передается методу `GetSupplierByCountry(country)` метод, где оно преобразуется в базу данных `NULL` значение `@Country` параметр в следующем запросе:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Выражение `Country = NULL` всегда возвращает значение False, даже для записей, `Country` столбец имеет `NULL` значением; поэтому записи не возвращаются.

Для возврата *все* поставщиков при пустом поле TextBox пустое, мы можем расширить `GetSuppliersByCountry(country)` в BLL для вызова `GetSuppliers()` метод при значении параметра страны `Nothing` и для вызова МЕТОДА `GetSuppliersByCountry(country)` в противном случае метод. Это окажет влияние возвращение всех поставщиков, если страна не указана и соответствующий набор поставщиков, если включен параметр страны.

Изменение `GetSuppliersByCountry(country)` метод в `SuppliersBLL` следующим образом:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Благодаря этому изменению `DeclarativeParams.aspx` странице будут показаны все поставщики при первом посещении (или каждый раз, когда `CountryName` пуст).


[![Все поставщики, теперь отображается по умолчанию](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Рис. 10**: все поставщики, теперь отображается по умолчанию ([Просмотр полноразмерного изображения](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Сводка

Для использования методов со входными параметрами, нам нужно указать значения для параметров в коллекции `SelectParameters` коллекции. Различные типы параметров позволяют значение параметра должны быть получены из различных источников. Тип параметра по умолчанию использует жестко запрограммированных значений, но так же, как легко (и без написания кода) значений параметров можно получить из строки запроса, переменные сеанса, файлы cookie и даже введенный пользователем значения из веб-элементов управления на странице.

На примерах, которые мы рассматривали в этом руководстве показано, как использование значений декларативных параметров. Однако иногда, иногда бывает необходимо использовать источник параметра недоступно, например текущую дату и время, или, если веб-узле используется членство, идентификатор пользователя посетителя. Для таких сценариев можно задать значения параметров программно до ObjectDataSource вызова метода базового объекта. Узнаете, как для этого в [следующему руководству](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Hilton giesenow). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](displaying-data-with-the-objectdatasource-vb.md)
> [Вперед](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
