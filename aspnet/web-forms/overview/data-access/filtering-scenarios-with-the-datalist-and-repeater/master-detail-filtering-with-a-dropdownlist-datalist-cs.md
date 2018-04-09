---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Фильтрация с помощью DropDownList (C#) иерархического | Документы Microsoft
author: rick-anderson
description: В этом учебнике показано, как отображать отчеты главного и подчиненного представлений одной веб-странице с помощью элементами управления DropDownList для вывода записей «master» и DataList displ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Иерархического Фильтрация с помощью DropDownList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) или [скачать PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> В этом учебнике показано, как отображать отчеты главного и подчиненного представлений одной веб-странице с помощью элементами управления DropDownList для отображения записей «главный» и DataList для отображения «подробности».


## <a name="introduction"></a>Вступление

Главная и подчиненная отчет, в котором сначала создан с помощью элемента управления GridView в предыдущем [иерархического фильтрации с DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) учебник, начинается с определенного набора записей «главный». Пользователь может детализировать в одну главную строку тем самым просмотр этой главной записи «подробности». Главная и подчиненная отчеты являются идеальным выбором для визуализации отношений один ко многим и для отображения подробных сведений из особенно «расширенный» таблиц (таблиц с большим числом столбцов). Мы изучили, как реализовать главного и подчиненного представлений отчетов с помощью элементов управления GridView и DetailsView в предыдущих учебниках. В этом учебнике и двух следующих мы будем повторную проверку этих концепций, но фокус на использование DataList и вместо управляет повторителя.

В этом учебнике мы рассмотрим использование DropDownList для содержат «главный» записи, с «сведения» записей, отображаемых в элементе управления DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Шаг 1: Добавление веб-страницы учебника главного и подчиненного представлений

Прежде чем начать этого учебника, сначала давайте немного добавить папки и страниц ASP.NET, которые потребуются для этого учебника и работе с главного и подчиненного представлений отчетов, с помощью элементов управления DataList и повторителя двух следующих. Начните с создания новой папки в проект с именем `DataListRepeaterFiltering`. Добавьте пять страниц ASP.NET в этой папке, в которых настроен на использование главной страницы `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Создание папки DataListRepeaterFiltering и добавление страницы учебника по ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Рис. 1**: создание `DataListRepeaterFiltering` папки и добавления страниц учебника по ASP.NET


Затем откройте `Default.aspx` страницы и перетащите `SectionLevelTutorialListing.ascx` пользовательского элемента управления с `UserControls` папку в область конструктора. Этот пользовательский элемент управления, который мы создали [главные страницы и переходов](../introduction/master-pages-and-site-navigation-cs.md) учебник, просматривает карту узла и отображает учебники из текущего раздела в виде маркированного списка.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**На рисунке 2**: добавление `SectionLevelTutorialListing.ascx` пользовательского элемента управления на `Default.aspx` ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Для отображения маркированного списка иерархического учебники, которые будут созданы, необходимо добавить их в карту узла. Откройте `Web.sitemap` файл и добавьте следующую разметку после разметку узел карты узла «Отображение данных с DataList и повторителя»:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Обновление карты сайта для включения новых страниц ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Рис. 3**: обновление карты сайта для включения новых страниц ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Шаг 2: Отображение категорий в DropDownList

Нашего главного и подчиненного представлений отчета будут отображаться категории в элементе управления DropDownList с продуктами выбранного элемента списка отображаются ниже на странице в DataList. Первой задачей, затем является отображение в элементе управления DropDownList категорий. Сначала откройте `FilterByDropDownList.aspx` страницы в `DataListRepeaterFiltering` папки и перетащите DropDownList из области элементов в конструктор страницы. Затем задайте DropDownList `ID` свойства `Categories`. Щелкните ссылку в смарт-теге DropDownList Выбор источника данных и создать новый элемент управления ObjectDataSource с именем `CategoriesDataSource`.


[![Добавить новый элемент управления ObjectDataSource CategoriesDataSource с именем](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Рис. 4**: добавить новый элемент управления ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Настройка нового ObjectDataSource таким образом, что он вызывает `CategoriesBLL` класса `GetCategories()` метод. После настройки ObjectDataSource мы по-прежнему необходимо указать поле источника данных, которые должны отображаться в раскрывающемся списке и которого должно быть связано как значение для каждого элемента списка. У `CategoryName` как отображение и `CategoryID` как значение для каждого элемента списка.


[![Иметь DropDownList отображения поля «Категория» и используйте CategoryID значение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Рис. 5**: У отображения DropDownList `CategoryName` и использует `CategoryID` как значение ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


На этом этапе у нас есть элемент управления DropDownList, заполненный записями из `Categories` таблицы (все действия выполняются за шесть секунд). Рис. 6 показан ход работы сих при просмотре через браузер.


[![Раскрывающийся список с текущими категориями](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Рис. 6**: раскрывающиеся списки текущие категории ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Шаг 2: Добавление DataList продуктов

В нашем отчете главного и подчиненного представлений остается перечислены продукты, связанные с выбранной категории. Для этого добавьте DataList страницы и создать новый элемент управления ObjectDataSource с именем `ProductsByCategoryDataSource`. У `ProductsByCategoryDataSource` управления извлечения данных из `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод. Поскольку в этом отчете главного и подчиненного представлений доступно только для чтения, выберите вариант (нет) на вкладках INSERT, UPDATE и DELETE.


[![Выберите метод GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Рис. 7**: выберите `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


После нажатия кнопки Далее, мастер ObjectDataSource запрашивает источник значения для `GetProductsByCategoryID(categoryID)` метода *`categoryID`* параметра. Чтобы использовать значение выбранного `categories` источника параметра установите элемент DropDownList для элемента управления и ControlID в `Categories`.


[![Присвоено значение из категории DropDownList categoryID параметра](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Рис. 8**: задать *`categoryID`* параметр в значение `Categories` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


После завершения работы мастера настройки источника данных, Visual Studio автоматически создаст `ItemTemplate` для DataList, в котором отображаются имя и значение каждого поля данных. Давайте улучшения DataList вместо этого использовать `ItemTemplate` , отображается только название продукта, категория, поставщик, количество на единицу и цены вместе с `SeparatorTemplate` , внедряет `<hr>` элемент между каждым элементом. Я буду использовать `ItemTemplate` из примера на [отображение данных с помощью DataList и элементы управления повторителем](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) учебника, но можно использовать любой шаблон разметки можно найти наиболее привлекательный.

После внесения этих изменений, DataList и его ObjectDataSource разметки должен выглядеть следующим образом:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Теперь пора просмотрите ход работы в браузере. При первом посещении страницы, эти продукты, принадлежащие к выбранной категории (Напитки) отображаются (как показано на рис. 9), но изменение DropDownList данные не обновляются. Это так, как для DataList для обновления требуется выполнить обратную передачу. Для этого мы можем либо задать DropDownList `AutoPostBack` свойства `true` или добавить кнопку веб-элемент управления на страницу. В этом учебнике мы удалили задать DropDownList `AutoPostBack` свойства `true`.

Рис иллюстрируют главного и подчиненного представлений отчета в действии.


[![При первом посещении страницы, отображаются продукты напитков](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Рис. 9**: при первом просмотре страницы, отображаются продукты напитки ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Выбор нового продукта (продукты) автоматически вызывает обратную передачу, обновление DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Рис. 10**: Выбор нового продукта (продукты) автоматически вызывает обратную передачу, обновление DataList ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Добавление элемента списка «--выберите категорию--»

При первом просмотре `FilterByDropDownList.aspx` страницы категорий, по умолчанию, показывающая продукты Напитки в DataList выбран первый элемент списка DropDownList (Напитки). В *иерархического фильтрации с DropDownList* учебника мы добавили это вариант «--выберите категорию--» в DropDownList, по умолчанию и, если флажок установлен, отображаются *все* из продукты в базе данных. Такой подход был управляемость при отображении списка продуктов в GridView, как каждая строка продукта заняло небольшого объема площадь экрана. Тем не менее, с DataList, сведения о каждом продукте используется гораздо большего размера фрагмента экрана. Давайте по-прежнему добавьте пункт «--выберите категорию--» и он выбран по умолчанию, но вместо отображения всех продуктов при выборе настроим ее, чтобы отобразить его ни одного продукта.

Чтобы добавить новый элемент списка DropDownList, перейдите в окне «Свойства» и щелкните на многоточие в `Items` свойство. Добавление элемента списка с `Text` «--выберите категорию--» и `Value` `0`.


![Добавить](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Рис. 11**: Добавление элемента списка «--выберите категорию--»


Кроме того можно добавить элемент списка, добавив следующую разметку DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Кроме того, необходимо установить элемент управления DropDownList `AppendDataBoundItems` для `true` поскольку если задано значение `false` (по умолчанию), категории привязаны к DropDownList из ObjectDataSource они заменят любые добавленные вручную списка элементы.


![Свойство AppendDataBoundItems имеет значение True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Рис. 12**: задать `AppendDataBoundItems` значение True


По причине мы выбрали значение `0` для списка «--выберите категорию--» элемента не так, как отображаются без категорий в системе со значением `0`, таким образом записи продукта не будет возвращаться при выборе элемента списка «--выберите категорию--». Чтобы проверить это, занять некоторое время, чтобы перейти на страницу с помощью браузера. Как показано на рисунке 13, при первоначальной Просмотр страницы выбран элемент списка «--выберите категорию--» и продукты не отображаются.


[![Когда](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Рис. 13**: при выборе элемента списка «--выберите категорию--» продукты не отображаются ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Если вместо этого будет отображать *все* продуктов, при выборе параметра «– выберите категорию –», используйте значение `-1` вместо него. Проницательный читатель будет помните, что назад в *иерархического фильтрации с DropDownList* учебника мы обновили `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод, чтобы если *`categoryID`* значение `-1` был передан в всех продуктов, возвращенных записей.

## <a name="summary"></a>Сводка

При отображении иерархически связанных данных часто полезно для представления данных с помощью главного и подчиненного представлений отчетов, из которых пользователь может начать изучение данных в верхней части иерархии и перейти к подробным сведениям. В этом учебнике было рассмотрено создание простого иерархического отчет, показывающий выбранной категории продуктов. Это осуществлялось с помощью элемента управления DropDownList для списка категорий и DataList для продуктов, принадлежащих выбранной категории.

В следующем уроке мы рассмотрим разделение записей master и подробные сведения на двух страницах. На первой странице список записей «главный» будет отображаться с ссылку, чтобы просмотреть подробные сведения. Если щелкнуть ссылку будет whisk пользователя на второй странице отображаются сведения для выбранной записи master.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было Рэнди Шмидт. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](master-detail-filtering-acess-two-pages-datalist-cs.md)
