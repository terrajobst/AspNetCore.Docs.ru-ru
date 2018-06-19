---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Иерархического фильтрацию с двумя элементами управления DropDownList (C#) | Документы Microsoft
author: rick-anderson
description: Этот учебник расширяется связь главного и подчиненного представлений для добавления третий уровень, с использованием двух элементов управления DropDownList для выбора нужного recor родительских и прародителя...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: d971dcb3814dc088202c3a3e4addb03375049ca0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887256"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Иерархического фильтрацию с двумя элементами управления DropDownList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) или [скачать PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Этот учебник расширяется связь главного и подчиненного представлений для добавления третий уровень, с использованием двух элементов управления DropDownList для выбора нужных записей родительских и прародителя.


## <a name="introduction"></a>Вступление

В [с предыдущим учебником](master-detail-filtering-with-a-dropdownlist-cs.md) мы рассмотрели, как отобразить простой основной подробности отчета с помощью одного DropDownList, заполненный категорий и GridView, показывающая те продукты, которые принадлежат выбранной категории. Этот шаблон отчета удобно использовать, когда Отображение записей, которые имеют связь «один ко многим» и может расширяться для работы сценариев, включающих несколько отношений один ко многим. Например система будет иметь таблицы, которые соответствуют клиентов, заказов и строк элементов заказа. Данный клиент может иметь несколько заказов с каждым заказом, состоящий из нескольких элементов. Такие данные могут быть представлены пользователю с двумя элементами управления DropDownList и GridView. Первый элемент DropDownList будет иметь элемент списка для каждого клиента в базе данных со вторым из них содержимое которой заказов выбранного клиента. Элемент управления GridView будет выведен список элементов строк из выбранного заказа.

Хотя базы данных Northwind включает канонические клиента или заказа сведений в его `Customers`, `Orders`, и `Order Details` таблицы, эти таблицы не заносятся в нашей архитектуры. Тем не менее по-прежнему можно проиллюстрировать с помощью двух зависимых элементами управления DropDownList. Первый элемент DropDownList выводит список категорий и второй продуктов, принадлежащих выбранной категории. DetailsView будут перечислены сведения о выбранном продукте.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Шаг 1: Создание и заполнение DropDownList категорий

Наша цель первого является добавление DropDownList, перечислены категории. Эти шаги подробно были проверены в предыдущем учебнике, но кратко перечислены ниже для полноты.

Откройте `MasterDetailsDetails.aspx` страницы в `Filtering` набор папок, добавьте на страницу DropDownList его `ID` свойства `Categories`и щелкните ссылку настроить источник данных в его смарт-тег. Мастер настройки источника данных выберите Добавление нового источника данных.


[![Добавить новый источник данных для DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Рис. 1**: добавить новый источник данных для DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


Новый источник данных естественно, следует ObjectDataSource. Назовите этот новый элемент управления ObjectDataSource `CategoriesDataSource` и его вызов `CategoriesBLL` объекта `GetCategories()` метод.


[![Выберите с помощью класса CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**На рисунке 2**: Выбор `CategoriesBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Настройка ObjectDataSource можно использовать метод GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Рис. 3**: Настройка ObjectDataSource для использования `GetCategories()` метод ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


После настройки ObjectDataSource нам по-прежнему необходимо указать, какие поля источника данных, которые должны отображаться в `Categories` DropDownList и какой из них должны быть настроены как значение элемента списка. Задать `CategoryName` как отображение и `CategoryID` как значение для каждого элемента списка.


[![Иметь DropDownList отображения поля «Категория» и используйте CategoryID значение](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Рис. 4**: У отображения DropDownList `CategoryName` и использует `CategoryID` как значение ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


На этом этапе у нас есть элемент управления DropDownList (`Categories`) заполняется записи из `Categories` таблицы. Когда пользователь выбирает категорию в выпадающем будем обратную передачу для обновления продукта DropDownList, мы создадим на шаге 2. Таким образом, установите флажок Включить AutoPostBack из `categories` DropDownList смарт-тега.


[![Включить AutoPostBack для DropDownList категорий](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Рис. 5**: Включение AutoPostBack для `Categories` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Шаг 2: Отображение выбранной категории продуктов в второй DropDownList

С `Categories` DropDownList завершена, следующим шагом является отображение DropDownList продуктов, относящихся к выбранной категории. Для этого добавьте другой элемент DropDownList на страницу с именем `ProductsByCategory`. Как и в `Categories` DropDownList, создайте новый элемент управления ObjectDataSource для `ProductsByCategory` DropDownList с именем `ProductsByCategoryDataSource`.


[![Добавить новый источник данных для ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Рис. 6**: добавить новый источник данных для `ProductsByCategory` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Создать новый элемент управления ObjectDataSource ProductsByCategoryDataSource с именем](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Рис. 7**: создать новый именованный ObjectDataSource `ProductsByCategoryDataSource` ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Поскольку `ProductsByCategory` DropDownList нужно отобразить только те продукты, принадлежащий к выбранной категории имеют ObjectDataSource вызова неуправляемого кода `GetProductsByCategoryID(categoryID)` метод `ProductsBLL` объекта.


[![Выберите с помощью класса ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Рис. 8**: Выбор `ProductsBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Настройка ObjectDataSource можно использовать метод GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Рис. 9**: Настройка ObjectDataSource для использования `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


На последнем шаге мастера необходимо указать значение *`categoryID`* параметра. Назначить этот параметр для выбранного элемента из `Categories` DropDownList.


[![В выпадающем категорий по запросу categoryID значение параметра](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Рис. 10**: по запросу *`categoryID`* значение параметра из `Categories` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Использование ObjectDataSource настроена все, что остается только указать, какие поля источника данных используются для отображения и значения элементов DropDownList. Отображение `ProductName` поля и использовать `ProductID` поля в качестве значения.


[![Укажите поля источника данных, используемые для текста и значение свойства DropDownList элементов ListItem](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Рис. 11**: Укажите поля источника данных используются для DropDownList `ListItem` s " `Text` и `Value` свойства ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Использование ObjectDataSource и `ProductsByCategory` DropDownList настроен нашу страницу отобразит двумя элементами управления DropDownList: первый будут перечислены все категории второй будет перечислять этих продуктов, принадлежащих выбранной категории. Когда пользователь выбирает категорию из первого DropDownList, произойдет обратная передача и второй DropDownList будет привязывается, показывающая те продукты, которые принадлежат к категории вновь выбранных. Фигуры 12 и 13 Показать `MasterDetailsDetails.aspx` в действии при просмотре через браузер.


[![При первом посещении страницы, выбрана категория напитков](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Рис. 12**: при первом просмотре страницы, выбрана категория напитков ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![При выборе другой категории отображаются новой категории продуктов](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Рис. 13**: Выбор новой категории продуктов на разных экранах категории ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


В настоящее время `productsByCategory` DropDownList при изменении *не* вызывает обратную передачу. Однако необходимо обратной передачи возникает, когда мы добавляем DetailsView для отображения сведений для выбранного продукта (шаг 3). Таким образом, установите флажок Включить AutoPostBack из `productsByCategory` DropDownList смарт-тега.


[![Включить функцию AutoPostBack для productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Рис. 14**: включить функцию AutoPostBack для `productsByCategory` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Шаг 3: С помощью DetailsView для отображения подробных сведений для выбранного продукта

Последний шаг — отображение сведений для выбранного продукта в DetailsView. Чтобы выполнить это, добавьте на страницу DetailsView, задайте его `ID` свойства `ProductDetails`и создать новый элемент управления ObjectDataSource для него. Настройте этот элемент управления ObjectDataSource для извлечения данных из `ProductsBLL` класса `GetProductByProductID(productID)` метода с помощью выбранного значения `ProductsByCategory` DropDownList для значения *`productID`* параметра.


[![Выберите с помощью класса ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Рис. 15**: Выбор `ProductsBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Настройка ObjectDataSource можно использовать метод GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**На рисунке 16**: Настройка ObjectDataSource для использования `GetProductByProductID(productID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Значение параметра productID по запросу из ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Рисунок 17**: по запросу *`productID`* значение параметра из `ProductsByCategory` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Вы можете получить сведения о доступных полей в DetailsView. Я решил удалить `ProductID`, `SupplierID`, и `CategoryID` полей и изменят порядок, а остальные поля в формате. Кроме того, снят out DetailsView `Height` и `Width` свойства, позволяя DetailsView развернуть по ширине, необходимые для отображения наиболее свои данные вместо того, он ограничен указанного размера. Полная разметка появляется ниже:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Занять некоторое время на ознакомление со `MasterDetailsDetails.aspx` страницы в браузере. На первый взгляд может показаться, что все работает в случае необходимости, но есть незначительные проблемы. При выборе новой категории `ProductsByCategory` DropDownList обновляется с учетом этих продуктов для выбранной категории, но `ProductDetails` DetailsView продолжает Показать предыдущие сведения о продукте. DetailsView обновляется при выборе другого продукта для выбранной категории. Кроме того, если достаточно тщательно протестировать, вы обнаружите, что при выборе постоянно новые категории (такие как выбор Напитки из `Categories` DropDownList, а затем приправы, затем Кондитерские изделия) каждой других выбранной категории вызывает `ProductDetails`DetailsView для обновления.

Чтобы конкретизировать эту проблему, давайте взглянем на конкретный пример. При первом посещении страницы выбрана категория напитков и связанные продукты загружаются в `ProductsByCategory` DropDownList. Chai является выбранного продукта и сведения о нем отображаются в `ProductDetails` DetailsView, как показано на рисунке 18.


[![Сведения о продукте выбран отображаются в элементе управления DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**На рисунке 18**: выбранного продукта сведения будут отображены в элементе управления DetailsView ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Происходит при изменении выбранной категории из Напитки приправы обратная передача и `ProductsByCategory` DropDownList обновляется соответствующим образом, но DetailsView по-прежнему отображаются сведения для товара.


[![Сведения о ранее выбранного продукта, по-прежнему отображаются](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**На рисунке 19**: ранее выбранного элемента сведения о продукте, по-прежнему отображаются ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Обновляет DetailsView комплектации новый продукт из списка, как ожидалось. Если вы выберете новую категорию после изменения продукта, DetailsView снова не обновляется. Тем не менее если вместо выбора нового продукта вы выбрали новой категории, будут обновлены DetailsView. В мире происходящем здесь?

Проблема заключается в жизненном цикле страницы ошибки синхронизации. При каждом запросе страницы, он проходит через ряд действий, как его подготовки к просмотру. Одно из следующих действий в ObjectDataSource управляет проверьте, если какие-либо их `SelectParameters` значения были изменены. Если таким образом, веб-управления, привязанные к ObjectDataSource знает, что необходимо обновить его отображения. Например, при выборе новой категории `ProductsByCategoryDataSource` ObjectDataSource обнаруживает, что его значения параметров были изменены и `ProductsByCategory` DropDownList число повторных привязок, получение продуктов для выбранной категории.

Проблема, которая возникает в этой ситуации происходит что точке жизненного цикла страницы, изменения параметров проверки ObjectDataSources *перед* перепривязки связанные данные веб-элементов управления. Таким образом при выборе новой категории `ProductsByCategoryDataSource` ObjectDataSource обнаруживает изменение в его параметр значение. Используемый элемент управления ObjectDataSource `ProductDetails` DetailsView, однако не Обратите внимание, какие-либо изменения из-за `ProductsByCategory` DropDownList еще не привязывается. Далее в жизненном цикле `ProductsByCategory` число повторных привязок DropDownList методы, перехватывая продукты для новой выбранной категории. Хотя `ProductsByCategory` DropDownList значение изменилось, `ProductDetails` DetailsView ObjectDataSource уже было произведено его проверить значение параметра; таким образом, DetailsView отобразит результаты предыдущего. Это взаимодействие показан на рис. 20.


[![Значение ProductsByCategory DropDownList изменяется после ObjectDataSource ProductDetails DetailsView проверяет наличие изменений](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Рис. 20**: `ProductsByCategory` изменений значение DropDownList после `ProductDetails` DetailsView ObjectDataSource проверяет наличие изменений ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Чтобы исправить это, необходимо явно повторно привязать `ProductDetails` DetailsView после `ProductsByCategory` DropDownList были связаны. Это можно сделать путем вызова `ProductDetails` DetailsView `DataBind()` метод при `ProductsByCategory` DropDownList `DataBound` вызывается событие. Добавьте следующий код обработчика событий для `MasterDetailsDetails.aspx` на странице кода класса (см. «[программно заданию значений параметров ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"сведения о том, как добавить обработчик событий):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

После этого явным вызовом `ProductDetails` DetailsView `DataBind()` был добавлен метод, работы с учебником, работают нормально. Выделяет рисунок 21, как это изменить исправить наших предыдущих проблему.


[![ProductDetails DetailsView является явным образом обновляется при ProductsByCategory DropDownList вызывается событие с привязкой к данным](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Рисунок 21**: `ProductDetails` DetailsView является явным образом обновляется при `ProductsByCategory` DropDownList `DataBound` события ([Просмотр полноразмерное изображение](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Сводка

DropDownList является идеальным элементом пользовательского интерфейса для главного и подчиненного представлений отчетов там, где есть один ко многим между главных и подчиненных записях. В предыдущем учебнике мы узнали, как использовать один DropDownList для фильтрации отображаемых по выбранной категории продуктов. В этом учебнике мы заменили GridView продуктов с DropDownList и используется для отображения сведений о выбранном продукте DetailsView. Концепции, описанные в этом учебнике можно легко расширить для моделей данных, включающих несколько связей один ко многим, например клиентов, заказов и порядок элементов. Как правило всегда можно добавить DropDownList для каждой сущности «один» в связи один ко многим.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Вперед](master-detail-filtering-across-two-pages-cs.md)
