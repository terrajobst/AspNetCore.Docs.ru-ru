---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: «Основной/подробности» Фильтрация с использованием двух элементов управления DropDownList (VB) | Документация Майкрософт
author: rick-anderson
description: Этот учебник расширяет отношение "основной/подробности" добавить третий уровень, с помощью двух элементов управления DropDownList для выбора нужного запи родительских и прародителя...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: e9921da99a61e0153446cf509fe1a6d8f0c39ec8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387781"
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>«Основной/подробности» Фильтрация с использованием двух элементов управления DropDownList (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) или [скачать PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> Этот учебник расширяется связь "основной/подробности" добавить третий уровень, с помощью двух элементов управления DropDownList для выбора нужных записей родительского и прародителя.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](master-detail-filtering-with-a-dropdownlist-vb.md) мы рассмотрели способ отображения простого обобщенных и подробных отчетов при помощи одного элемента управления DropDownList, заполненный категорий и GridView, отображаются продукты, относящиеся к выбранной категории. Этот шаблон отчета работает также в том случае, при отображении записей, которые имеют связь «один ко многим» и можно легко расширить для работы сценариев, включающих несколько связей один ко многим. Например система ввода заказов бы таблицы, соответствующие клиенты заказов и строк элементов заказа. У данного клиента может быть несколько заказов с каждым заказом, состоящий из нескольких элементов. Такие данные могут быть представлены пользователю при двух элементов управления DropDownList и GridView. Первый DropDownList будет иметь элемент списка для каждого клиента в базе данных со вторым из них содержимое которого заказы, размещенные выбранного клиента. GridView перечислял линейные элементы из выбранного заказа.

Хотя базы данных Northwind включают в себя сведения каноническую сведения клиента или заказа, в его `Customers`, `Orders`, и `Order Details` таблицы, эти таблицы не фиксируются в архитектуре. Тем не менее по-прежнему можно проиллюстрировать с помощью двух зависимых элементов управления DropDownList. Первый DropDownList выводит список категорий и второй продуктов, принадлежащих выбранной категории. Элементе управления DetailsView затем будут содержать сведения о выбранном продукте.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Шаг 1: Создание и заполнение элемента управления DropDownList категории

Наша первая цель является добавление элемента управления DropDownList, перечислены категории. Эти действия были подробно рассматриваемую в предыдущем учебном курсе, но кратко перечислены ниже для полноты картины.

Откройте `MasterDetailsDetails.aspx` странице в `Filtering` Добавление элемента управления DropDownList на страницу набор папок, его `ID` свойства `Categories`и затем щелкните ссылку Настройка источника данных в его смарт-тега. В мастере настройки источника данных выберите для добавления нового источника данных.


[![Добавить новый источник данных для элемента управления DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Рис. 1**: добавить новый источник данных для элемента управления DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


Новый источник данных естественно, следует элементу ObjectDataSource. Назовите этот новый элемент управления ObjectDataSource `CategoriesDataSource` , который будет вызывать `CategoriesBLL` объекта `GetCategories()` метод.


[![Выберите для использования класса](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Рис. 2**: Выбор `CategoriesBLL` класс ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Настройте элемент ObjectDataSource для использования метода GetCategories()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Рис. 3**: Настройка ObjectDataSource для использования `GetCategories()` метод ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


После настройки ObjectDataSource все равно небходимо указать поле из источника данных, которые должны отображаться в `Categories` DropDownList и какой из них должны быть настроены в качестве значения для элемента списка. Задайте `CategoryName` как отображение и `CategoryID` как значение для каждого элемента списка.


[![Иметь элемент управления DropDownList отображает поле «Категория» и CategoryID использовать как значение](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Рис. 4**: иметь элемент управления DropDownList отображает `CategoryName` и использует `CategoryID` как значение ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


На этом этапе у нас есть элемент управления DropDownList (`Categories`) который заполняется записи из `Categories` таблицы. Когда пользователь выбирает новую категорию в элементе управления DropDownList, нам понадобятся обратную передачу для обновления продукта DropDownList, который мы собираемся создать на шаге 2. Таким образом, установите флажок Включить AutoPostBack из `categories` смарт-тега DropDownList.


[![Включить AutoPostBack для элемента управления DropDownList категории](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Рис. 5**: включить AutoPostBack для `Categories` DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Шаг 2: Отображение продуктов выбранной категории во втором DropDownList

С помощью `Categories` завершения DropDownList, следующим этапом является отображение элемента управления DropDownList продуктов, принадлежащих выбранной категории. Для этого необходимо добавить другой DropDownList на страницу с именем `ProductsByCategory`. Как и в `Categories` DropDownList, создайте новый ObjectDataSource для `ProductsByCategory` DropDownList с именем `ProductsByCategoryDataSource`.


[![Добавить новый источник данных для элемента управления ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Рис. 6**: добавить новый источник данных для `ProductsByCategory` DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Создайте новый ObjectDataSource, именуемый ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Рис. 7**: создайте новый ObjectDataSource с именем `ProductsByCategoryDataSource` ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Так как `ProductsByCategory` потребностей DropDownList для отображения только тех продуктов, принадлежащих выбранной категории, имеют вызвать элемент управления ObjectDataSource `GetProductsByCategoryID(categoryID)` метода из `ProductsBLL` объекта.


[![Выберите использование класса ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Рис. 8**: Выбор `ProductsBLL` класс ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Настройка ObjectDataSource на использование метод GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Рис. 9**: Настройка ObjectDataSource для использования `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


На последнем шаге мастера необходимо указать значение *`categoryID`* параметра. Назначить этот параметр для выбранного элемента из `Categories` DropDownList.


[![По запросу categoryID значение параметра в элементе управления DropDownList категории](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Рис. 10**: по запросу *`categoryID`* значение параметра из `Categories` DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


С помощью ObjectDataSource настроен остается только для задания поля источника данных, используемые для отображения и значения элементов управления DropDownList. Отображение `ProductName` поле и использовать `ProductID` как значение.


[![Укажите поля источника данных, используемые для текста и значение свойства DropDownList ListItems](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Рис. 11**: укажите используемые поля источника данных для элемента управления DropDownList `ListItem` s " `Text` и `Value` свойства ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


С помощью ObjectDataSource и `ProductsByCategory` DropDownList настроен нашей странице будет отображаться двух элементов управления DropDownList: первый будут перечислены все категории во время второй будут перечислены продукты, принадлежащие к выбранной категории. Когда пользователь выбирает новую категорию из первого элемента управления DropDownList, произойдет обратная передача и втором DropDownList будет повторную привязку, показывающий продукты, относящиеся к новой выбранной категории. Рис. 12 и 13 show `MasterDetailsDetails.aspx` в действии при просмотре через браузер.


[![При первом просмотре странице, выбирается категории «Напитки»](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Рис. 12**: выбран при первом просмотре страницы, категории «Напитки» ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![При выборе другой категории отображаются новой категории продуктов](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Рис. 13**: Выбор новой категории продуктов на разных экранах категории ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


В настоящее время `productsByCategory` DropDownList, при изменении *не* вызывает обратную передачу. Однако нам обратной передачи возникает, когда мы добавляем в элементе управления DetailsView для отображения сведений о выбранном продукте (шаг 3). Таким образом, установите флажок Включить AutoPostBack из `productsByCategory` смарт-тега DropDownList.


[![Включить функцию AutoPostBack для productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Рис. 14**: включить функцию AutoPostBack для `productsByCategory` DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Шаг 3: Использование элементе управления DetailsView для отображения сведений о выбранном продукте

Последним шагом является отображение подробные сведения о выбранном продукте в элементе управления DetailsView. Добавление этого в элементе управления DetailsView на страницу, задайте его `ID` свойства `ProductDetails`и создайте новый ObjectDataSource, для него. Настроить данный элемент управления ObjectDataSource для извлечения данных из `ProductsBLL` класса `GetProductByProductID(productID)` метод, используя выбранное значение `ProductsByCategory` DropDownList для значения *`productID`* параметра.


[![Выберите использование класса ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Рис. 15**: Выбор `ProductsBLL` класс ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Настройка ObjectDataSource на использование метода GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Рис. 16**: Настройка ObjectDataSource для использования `GetProductByProductID(productID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![По запросу productID значение параметра в элементе управления ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Рис. 17**: по запросу *`productID`* значение параметра из `ProductsByCategory` DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Можно выбрать для отображения всех доступных полей в `ProductDetails` DetailsView. Мы решили удалить `ProductID`, `SupplierID`, и `CategoryID` поля и изменят порядок, а остальные поля в формате. Кроме того, я очищаются DetailsView `Height` и `Width` свойств, что позволяет элементу управления DetailsView для расширения к ширине, необходимой для наилучшего отображения данных, вместо того, он ограничен заданного размера. Полная разметка androi:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Отвлекитесь и опробовать `MasterDetailsDetails.aspx` страницу в браузере. На первый взгляд может показаться, что все работает при необходимости, но есть деталь. При выборе новой категории `ProductsByCategory` DropDownList обновляется для включения этих продуктов для выбранной категории, но `ProductDetails` продолжение DetailsView для отображения предыдущей информации о продукте. DetailsView обновляется, при выборе другого продукта для выбранной категории. Кроме того, если вы достаточно тщательно протестировать, вы обнаружите, что при выборе постоянно новые категории (например, при выборе «Напитки» из `Categories` DropDownList, а затем Специи, затем Кондитерские изделия) каждые других выбранной категории вызывает `ProductDetails`DetailsView для обновления.

Чтобы помочь удается конкретизировать эту проблему, давайте рассмотрим конкретный пример. При первом посещении страницы выбранные категории «Напитки», и ее использовании связанные продукты загружаются в `ProductsByCategory` DropDownList. Chai является выбранного продукта и сведения о нем отображаются в `ProductDetails` DetailsView, как показано на рис. 18.


[![Сведения о продукте выбран, отображаются в элементе управления DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Рис. 18**: выбранные сведения о продукте отображаются в элементе управления DetailsView ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Если изменить выбор категории «Напитки» на «Специи», происходит обратная передача и `ProductsByCategory` DropDownList обновляется соответствующим образом, но DetailsView по-прежнему отображаются сведения для продукта Chai.


[![Ранее выбранные сведения о продукте, по-прежнему отображаться](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Рис. 19**: ранее выбранные сведения о продукте, по-прежнему отображаются ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


Выбор нового продукта в списке обновляет DetailsView, должным образом. Если вы выберете новую категорию после изменения продукта, DetailsView снова не обновляется. Тем не менее если вместо выбора нового продукта вы выбрали новую категорию, будут обновлены DetailsView. В мире происходящего здесь?

Проблема заключается в жизненном цикле страницы ошибки синхронизации. Каждый раз, когда страница запрашивается, что проходит несколько этапов, как его подготовки к просмотру. Проверьте, если какие-либо одно из следующих действий управляет элемент управления ObjectDataSource их `SelectParameters` значения изменились. Если таким образом, веб-элемента управления данных привязано к ObjectDataSource знает, что необходимо обновить его отображение. Например, при выборе новой категории `ProductsByCategoryDataSource` ObjectDataSource обнаруживает, что его значения параметров были изменены и `ProductsByCategory` DropDownList повторную привязку, получить продукты для выбранной категории.

Сформированной в этой ситуации проблема в том, что точка в жизненном цикле страницы, который элементов управления ObjectDataSource проверять изменения параметров происходит *перед* повторная привязка связанных данных веб-элементов управления. Таким образом при выборе новой категории `ProductsByCategoryDataSource` ObjectDataSource обнаруживает изменение в значении параметра. Элемент управления ObjectDataSource, используемые `ProductDetails` DetailsView, обратите внимание, не всех изменениях, поскольку `ProductsByCategory` DropDownList еще привязывается. Далее в жизненный цикл `ProductsByCategory` выполняет повторную привязку элемента управления DropDownList управления ObjectDataSource, получение продуктов для новой выбранной категории. Хотя `ProductsByCategory` DropDownList значение изменилось, `ProductDetails` DetailsView ObjectDataSource уже выполнил его проверить значение параметра; таким образом, элемент DetailsView отображает предыдущие результаты. Это взаимодействие показан на рис. 20.


[![Значение элемента управления ProductsByCategory DropDownList изменяется после ObjectDataSource ProductDetails DetailsView проверяет наличие изменений](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Рис. 20**: `ProductsByCategory` изменения значения элемента управления DropDownList после `ProductDetails` DetailsView ObjectDataSource, проверяет наличие изменений ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Чтобы исправить это нам нужно явное повторное привязывание `ProductDetails` DetailsView после `ProductsByCategory` привязанного элемента управления DropDownList. Это можно сделать, вызвав `ProductDetails` DetailsView `DataBind()` метод при `ProductsByCategory` DropDownList `DataBound` вызывает событие. Добавьте следующий код обработчика событий к `MasterDetailsDetails.aspx` вспомогательном классе страницы (см. «[Программная установка значений параметров ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"сведения о том, как добавить обработчик событий):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

После этого явным вызовом `ProductDetails` DetailsView `DataBind()` был добавлен метод, учебника работает должным образом. Рис. 21 основные особенности, как это изменить устранено возникли проблемы с более ранней.


[![ProductDetails DetailsView не вызывает событие DataBound явным образом обновляется при ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Рис. 21**: `ProductDetails` DetailsView не явным образом обновляется при `ProductsByCategory` DropDownList `DataBound` вызывает событие ([Просмотр полноразмерного изображения](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Сводка

DropDownList является идеальным элементом пользовательского интерфейса для отчетов «основной/подробности» там, где действует связь один ко многим между основными и подробными записями. В предыдущем учебном курсе мы рассмотрели использование одного элемента управления DropDownList для фильтрации товаров, отображаемого элементом в выбранной категории. В этом руководстве мы заменили элемента управления GridView для продуктов на элементе управления DropDownList, а используемый элементе управления DetailsView для отображения сведений о выбранном продукте. Понятия, рассматриваемые в этом руководстве можно легко расширить к моделям данных, с участием нескольких связей один ко многим, таких как клиенты, заказы и элементы заказа. Как правило всегда можно добавить элемента управления DropDownList для каждой сущности «один» в связи один ко многим.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Hilton giesenow). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Вперед](master-detail-filtering-across-two-pages-vb.md)
