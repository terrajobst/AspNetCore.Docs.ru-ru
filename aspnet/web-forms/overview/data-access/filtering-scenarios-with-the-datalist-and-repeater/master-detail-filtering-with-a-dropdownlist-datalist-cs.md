---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Основной/подробности Фильтрация с помощью элемента управления DropDownList (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве показано, как отображать отчеты «основной/подробности» на одной веб-странице, используя списки DropDownList для отображения «master» записей и элемент управления DataList для displ...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed2631e49786c81075099cca6941d98ba3b67e37
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818256"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Основной/подробности Фильтрация с помощью элемента управления DropDownList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) или [скачать PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> В этом руководстве показано, как отображать отчеты «основной/подробности» на одной веб-странице, используя списки DropDownList для отображения «основных» записей и элемент управления DataList для «подробностей».


## <a name="introduction"></a>Вступление

Отчет «основной/подробности», который мы впервые создали с помощью элемента управления GridView в более ранней ["основной/подробности" Фильтрация с помощью элемента управления DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) руководства начинается с демонстрации определенного набора «основных» записей. Пользователь может затем перейти к одной из основных записей, таким образом просматривая основной записи «подробности». Отчеты «основной/подробности» являются идеальным выбором для визуализации отношений «один ко многим», а также для отображения подробных сведений из особенно «широких» таблиц (таблиц со множеством столбцов). Курсах было продемонстрировано, как реализовать отчетов «основной/подробности», с помощью элементов управления GridView и DetailsView в предыдущих учебных курсах. В этом руководстве и следующих двух будут изучены эти понятия, но внимание уделяется использованию элементов управления DataList и Repeater элементы управления.

В этом руководстве мы рассмотрим использование элемента управления DropDownList для размещения «основных» записей, с записями «сведения», отображаемый в элементе управления DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Шаг 1: Добавление веб-страниц учебного "основной/подробности"

Прежде чем приступать к этом руководстве, давайте добавим папку страницы ASP.NET, которые потребуются для этого учебника и следующих двух дело отчеты «основной/подробности» с помощью элементов управления DataList и Repeater некоторое время. Начнем с создания новой папки в проект с именем `DataListRepeaterFiltering`. Добавьте следующие пять страниц ASP.NET в этой папке, в которых настроено использование главной страницы `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Создание папки DataListRepeaterFiltering и добавление страниц учебника по ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Рис. 1**: создание `DataListRepeaterFiltering` папки и добавление страниц учебника по ASP.NET


Затем откройте `Default.aspx` страницы и перетащите `SectionLevelTutorialListing.ascx` пользовательского элемента управления с `UserControls` папку в область конструктора. Данный пользовательский элемент управления, созданный в учебном курсе [главные страницы и структуру переходов узла](../introduction/master-pages-and-site-navigation-cs.md) , просматривает карту узла и отображает руководства из текущего раздела в виде маркированного списка.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Рис. 2**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Чтобы маркированный список отображал руководства «основной/подробности», которые будут созданы, необходимо добавить их в карту узла. Откройте `Web.sitemap` файл и добавьте следующую разметку после разметки узла карты сайта «Отображение данных с помощью элементов управления DataList и Repeater»:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Обновление карты узла для добавления новых страниц ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Рис. 3**: обновите карту узла, чтобы включить новые страницы ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Шаг 2: Отображение категорий в элементе управления DropDownList

Категории в элементе управления DropDownList, перечислит нашего отчета «основной/подробности» с продуктами выбранного элемента списка отображаются ниже на странице в элементе управления DataList. Первой задачей, то категорий, отображаемых в элементе управления DropDownList. Сначала откройте `FilterByDropDownList.aspx` странице в `DataListRepeaterFiltering` папки и перетащите DropDownList с панели элементов в конструктор страницы. Затем задайте DropDownList `ID` свойства `Categories`. Щелкните ссылку выберите источник данных смарт-теге DropDownList и создайте новый ObjectDataSource, именуемый `CategoriesDataSource`.


[![Добавьте новый ObjectDataSource, именуемый CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Рис. 4**: добавьте новый ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Настройка нового ObjectDataSource, таким образом, он вызывает `CategoriesBLL` класса `GetCategories()` метод. После настройки ObjectDataSource все равно небходимо указать поле источника данных, которые должны отображаться в DropDownList, а какие должно быть связано в качестве значения для каждого элемента списка. У `CategoryName` как отображение и `CategoryID` как значение для каждого элемента списка.


[![Иметь элемент управления DropDownList отображает поле «Категория» и CategoryID использовать как значение](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Рис. 5**: иметь элемент управления DropDownList отображает `CategoryName` и использует `CategoryID` как значение ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


На этом этапе у нас есть элемент управления DropDownList, заполненный записями из `Categories` (все действия выполняются приблизительно за шесть секунд). Рис. 6 показаны до сих при просмотре через браузер.


[![Раскрывающийся список с текущими категориями](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Рис. 6**: раскрывающийся список текущих категорий ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Шаг 2: Добавление элементов управления DataList продуктов

Последний шаг в нашем отчете «основной/подробности» является отображение списка продуктов, связанных с выбранной категорией. Для этого добавьте на страницу элемент управления DataList и создайте новый ObjectDataSource, именуемый `ProductsByCategoryDataSource`. У `ProductsByCategoryDataSource` получит свои данные из `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод. Так как этот отчет «основной/подробности» доступна только для чтения, выберите параметр (нет) на вкладках INSERT, UPDATE и DELETE.


[![Выберите метод GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Рис. 7**: выберите `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


После нажатия кнопки Далее, мастер ObjectDataSource запрашивает значения для источника `GetProductsByCategoryID(categoryID)` метода *`categoryID`* параметра. Чтобы использовать значение выбранного `categories` элемент DropDownList источника параметра установите для элемента управления, а для ControlID – `Categories`.


[![Значение параметра categoryID значение DropDownList категорий](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Рис. 8**: задать *`categoryID`* параметр значению `Categories` DropDownList ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


После завершения работы мастера настройки источника данных, Visual Studio автоматически создаст `ItemTemplate` элемента управления DataList, отображающий имя и значение каждого поля данных. Давайте улучшим элемент управления DataList, чтобы вместо этого использовать `ItemTemplate` , отображающий только название продукта, категории, поставщика, количество в единицах измерения и цену, вместе с `SeparatorTemplate` , вводящим `<hr>` между вышеуказанными элементами. Я буду использовать `ItemTemplate` из примера в [отображение данных с помощью элементов управления DataList и Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) руководство, но вы можете использовать любые разметку шаблона, вы кажется наиболее визуально привлекательной.

После внесения этих изменений, DataList и его ObjectDataSource разметки должен выглядеть следующим образом:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Отвлекитесь и просмотрите ход работы в браузере. При первом просмотре странице, продукты, принадлежащие к выбранной категории (Напитки) отображаются (как показано на рис. 9), но изменения в элемент управления DropDownList данные не обновляются. Это обусловлено тем, для обновления элемента управления DataList необходимо осуществление обратной передачи. Для этого мы можем либо задать DropDownList `AutoPostBack` свойства `true` или добавить кнопку веб-элемент управления на страницу. В этом учебнике я выбрал установку DropDownList `AutoPostBack` свойства `true`.

На рис. 9 и 10 показана работа отчета «основной/подробности» в действии.


[![При первом просмотре странице отображаются продукты отображаются](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Рис. 9**: отображаются при первом просмотре странице отображаются продукты ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Выбор нового продукта (Produce) автоматически вызывает обратную передачу, обновляется элемент управления DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Рис. 10**: Выбор нового продукта (Produce) автоматически вызывает обратную передачу, обновляется элемент управления DataList ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Добавление элемента списка «--выберите категорию--»

При первом просмотре `FilterByDropDownList.aspx` странице первый элемент списка DropDownList (Напитки) выбран по умолчанию, показывающие в DataList отображаются продукты категории. В *"основной/подробности" Фильтрация с помощью элемента управления DropDownList* руководстве, мы добавили параметр «--выберите категорию--» в элемент управления DropDownList, которое был выбран по умолчанию и, при выборе выводится *все* из продукты в базе данных. Такой подход был управляем при выводе продуктов в элементе управления GridView, как каждый продукт занимал малого объема пространства экрана. Элемент управления DataList Однако сведения о каждом продукте занимает гораздо больший кусок экрана. Давайте по-прежнему добавьте пункт «--выберите категорию--» и установите его включенным по умолчанию, но вместо показа всех продуктов при выборе, настройте его таким образом, чтобы он показывал бы продукты.

Чтобы добавить новый элемент списка DropDownList, перейдите в окно свойств и щелкните эллипсы в `Items` свойство. Добавить новый элемент списка с `Text` «--выберите категорию--» и `Value` `0`.


![Добавить](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Рис. 11**: Добавление элемента списка «--выберите категорию--»


Кроме того можно добавить элемент списка, добавив следующую разметку в элемент управления DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Кроме того, нам нужно установить элемент управления DropDownList `AppendDataBoundItems` для `true` потому что если он становится равным `false` (по умолчанию), при привязке категорий к DropDownList из ObjectDataSource они будут перезаписывать все списки, добавленные вручную элементы.


![Установите для свойства AppendDataBoundItems значение true](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Рис. 12**: задать `AppendDataBoundItems` присваивается значение True


Причина, по значение `0` для списка «--выберите категорию--» элемента обеспечивается, так как в системе со значением категории не `0`, поэтому записи не продукта будет возвращаться при выборе элемента списка «--выберите категорию--». Чтобы проверить это, Отвлекитесь и чтобы перейти на страницу через обозреватель. Как показано на рисунке 13, при первоначальном просмотре страницы выбран элемент списка «--выберите категорию--» продукты не отображаются.


[![Когда](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Рис. 13**: при выборе элемента списка «--выберите категорию--» продукты не отображаются ([Просмотр полноразмерного изображения](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Если предпочтительнее отобразить *все* продуктов, при выборе параметра «--выберите категорию--», используйте значение `-1` вместо этого. Внимательный читатель, наверное, помните, назад в *"основной/подробности" Фильтрация с помощью элемента управления DropDownList* руководстве, мы обновили `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод таким образом, если *`categoryID`* значение `-1` был передан в всех продуктов, возвращенных записей.

## <a name="summary"></a>Сводка

При отображении иерархически связанных данных часто полезно представлять данные с помощью отчетов «основной/подробности», из которых пользователь может начать изучение данных в верхней части иерархии и перейти к подробным сведениям. В этом руководстве было рассмотрено создание "основной/подробности" простой отчет, показывающий продукты выбранной категории. Это осуществлялось с помощью элемента управления DropDownList для списка категорий и DataList для продуктов, принадлежащих выбранной категории.

В следующем учебном курсе мы рассмотрим разделение записей на двух страницах. На первой странице список «основных» записей будет отображаться со ссылкой для получения сведений. Щелчок ссылки перенесет пользователя на вторую страницу, который будет отображать подробные сведения о выбранной основной записи.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного Рэнди Шмидт. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](master-detail-filtering-acess-two-pages-datalist-cs.md)
