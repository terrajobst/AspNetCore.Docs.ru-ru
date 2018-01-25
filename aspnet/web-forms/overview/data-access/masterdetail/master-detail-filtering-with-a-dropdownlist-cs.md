---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: "Фильтрация с помощью DropDownList (C#) иерархического | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим отображение основных записей в элемент управления DropDownList и сведений для выбранного элемента списка в элементе управления GridView."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: cf3058ac095bc2ed728a716e70f962e260eef5a2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Иерархического Фильтрация с помощью DropDownList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) или [скачать PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> В этом учебнике мы рассмотрим отображение основных записей в элемент управления DropDownList и сведений для выбранного элемента списка в элементе управления GridView.


## <a name="introduction"></a>Вступление

Распространенным типом отчета является *главного и подчиненного представлений отчетов*, в который начинается с определенного набора записей «главный». Пользователь может детализировать в одну главную строку тем самым просмотр этой главной записи «подробности». Главная и подчиненная отчеты являются идеальным выбором для визуализации отношений один ко многим, например отчета показаны все категории и позволяющего пользователю выбрать определенную категорию и отобразить соответствующие продукты. Кроме того иерархического отчеты полезны для отображения подробных сведений из чрезвычайно «широких» таблиц (таблиц с большим числом столбцов). Например, «главный» уровень главного и подчиненного представлений отчета может показывать только имя и единицы цену продукта продуктов в базе данных и детализация конкретного продукта будут показаны дополнительные поля продукта (категория, поставщик, количество на единицу, а т. д).

Существует множество способов, с помощью которых можно реализовать главного и подчиненного представлений отчета. В этом и следующих трех учебных мы рассмотрим различные отчеты главного и подчиненного представлений. В этом учебнике мы рассмотрим отображение основных записей в [управления DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) и сведения для выбранного элемента списка в элементе управления GridView. В частности этот учебник главного и подчиненного представлений отчета будут отображаться сведения о категории и продукта.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Шаг 1: Отображение категорий в DropDownList

Нашего главного и подчиненного представлений отчета будут отображаться категории в элементе управления DropDownList с продуктами выбранного элемента списка отображаются ниже на странице в элементе управления GridView. Первой задачей, затем является отображение в элементе управления DropDownList категорий. Откройте `FilterByDropDownList.aspx` страницы в `Filtering` , перетащите DropDownList из области элементов в конструктор страницы, а значение его `ID` свойства `Categories`. Затем щелкните ссылку Выбор источника данных из DropDownList смарт-тега. Появится мастер настройки источника данных.


[![Укажите источник данных DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Рис. 1**: Укажите источник данных DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Выберите Добавить новый элемент управления ObjectDataSource с именем `CategoriesDataSource` , вызывающий `CategoriesBLL` класса `GetCategories()` метод.


[![Добавить новый элемент управления ObjectDataSource CategoriesDataSource с именем](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**На рисунке 2**: добавить новый элемент управления ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![Выберите с помощью класса CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Рис. 3**: Выбор `CategoriesBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![Настройка ObjectDataSource можно использовать метод GetCategories()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Рис. 4**: Настройка ObjectDataSource для использования `GetCategories()` метод ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


После настройки ObjectDataSource мы по-прежнему необходимо указать поле источника данных, которые должны отображаться в DropDownList и которого должно быть связано как значение элемента списка. У `CategoryName` как отображение и `CategoryID` как значение для каждого элемента списка.


[![Иметь DropDownList отображения поля «Категория» и используйте CategoryID значение](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Рис. 5**: У отображения DropDownList `CategoryName` и использует `CategoryID` как значение ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


На этом этапе у нас есть элемент управления DropDownList, заполненный записями из `Categories` таблицы (все действия выполняются за шесть секунд). Рис. 6 показан ход работы сих при просмотре через браузер.


[![Раскрывающийся список с текущими категориями](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Рис. 6**: раскрывающиеся списки текущие категории ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Шаг 2: Добавление элемента GridView с продуктами

Этот последний шаг в отчете иерархического является список продуктов, связанных с выбранной категории. Для этого добавьте на страницу элемент управления GridView и создать новый элемент управления ObjectDataSource с именем `productsDataSource`. У `productsDataSource` должен получать данные от `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод.


[![Выберите метод GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Рис. 7**: выберите `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


После выбора этого метода мастер ObjectDataSource запрашивает значения для метода  *`categoryID`*  параметра. Чтобы использовать значение выбранного `categories` источника параметра установите элемент DropDownList для элемента управления и ControlID в `Categories`.


[![Присвоено значение из категории DropDownList categoryID параметра](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Рис. 8**: задать  *`categoryID`*  параметр в значение `Categories` DropDownList ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Теперь пора просмотрите ход работы в браузере. При первом посещении страницы, эти продукты принадлежат выбранной категории (Напитки) отображаются (как показано на рис. 9), но изменение DropDownList данные не обновляются. Это так, как для GridView для обновления требуется выполнить обратную передачу. Для этого у нас есть два варианта (ни один из которых требует написания кода):

- **Задать категории DropDownList в**[свойство AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**значение True.** (Это можно сделать, установив параметр включения AutoPostBack в DropDownList смарт-тег.) Обратная передача будет вызываться каждый раз при выделении DropDownList изменении элемента пользователем. Таким образом когда пользователь выбирает категорию в выпадающем произойдет обратная передача и GridView будет обновляться с продуктами для новой выбранной категории. (Это подход, который использовался в этом учебнике).
- **Добавьте кнопку веб-элемент управления рядом с DropDownList.** Задайте его `Text` свойства для обновления или что-нибудь подобное. При таком подходе пользователю могут потребоваться для выбора новой категории, а затем нажмите кнопку. Нажмите кнопку вызывает обратную передачу и обновление GridView отображены продукты выбранной категории.

Рис иллюстрируют главного и подчиненного представлений отчета в действии.


[![При первом посещении страницы, отображаются продукты напитков](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Рис. 9**: при первом просмотре страницы, отображаются продукты напитки ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Выбор нового продукта (продукты) автоматически вызывает обратную передачу, обновляется GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Рис. 10**: Выбор нового продукта (продукты) автоматически вызывает обратную передачу, обновляется GridView ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Добавление элемента списка «--выберите категорию--»

При первом просмотре `FilterByDropDownList.aspx` страницы категорий, по умолчанию, отображающий продукты напитков в GridView выбран первый элемент списка DropDownList (Напитки). Вместо отображения первой категории продуктов, необходимо вместо имеют элемент DropDownList выбран, названием наподобие «--выберите категорию--».

Чтобы добавить новый элемент списка DropDownList, перейдите в окне «Свойства» и щелкните на многоточие в `Items` свойство. Добавление элемента списка с `Text` «--выберите категорию--» и `Value` `-1`.


[![Добавить--выберите категорию--элемента списка](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Рис. 11**: Добавление--выберите категорию--элемента списка ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


Кроме того можно добавить элемент списка, добавив следующую разметку DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Кроме того, необходимо установить элемент управления DropDownList `AppendDataBoundItems` значение True, так как при категории привязаны к DropDownList из ObjectDataSource они заменят любые элементы списка, добавленные вручную, если `AppendDataBoundItems` не установлено значение True.


![Свойство AppendDataBoundItems имеет значение True](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Рис. 12**: задать `AppendDataBoundItems` значение True


После внесения этих изменений, при первом посещении страницы выбран параметр «– выберите категорию –» и продукты не отображаются.


[![При загрузке начальной страницы продукты не отображаются](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Рис. 13**: на начальной странице нагрузки нет продукты отображаются ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


Продукты не отображаются при, поскольку выбран элемент списка «--выберите категорию--», так как его значение равно `-1` и нет ни одного продукта в базе данных с `CategoryID` из `-1`. Если это поведение необходимо, то в этот момент завершения операции! Если, однако требуется отобразить *все* категорий при выборе элемента списка «--выберите категорию--» вернуться к `ProductsBLL` и настройте `GetProductsByCategoryID(categoryID)` метод, чтобы он вызывал `GetProducts()` метод Если переданный в  *`categoryID`*  меньше нуля:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

Методика аналогична приемом, использованным для отображения всех поставщиков в [декларативные параметры](../basic-reporting/declarative-parameters-cs.md) учебника, несмотря на то, что в этом примере мы используем значение `-1` для указания, что все записи должно быть в отличие от получения `null`. Это вызвано  *`categoryID`*  параметр `GetProductsByCategoryID(categoryID)` метод ожидает как целочисленное значение, переданное в, в то время как в этом учебнике декларативные параметры мы передавался строковый входной параметр.

Снимок экрана показано на рис. 14 `FilterByDropDownList.aspx` при выборе параметра «--выберите категорию--». Здесь по умолчанию отображаются все продукты, и пользователь может сузить отображения, выбрав определенную категорию.


[![Все продукты, теперь в списке по умолчанию](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Рис. 14**: все продукты, теперь в списке по умолчанию ([Просмотр полноразмерное изображение](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Сводка

При отображении иерархически связанных данных часто полезно для представления данных с помощью главного и подчиненного представлений отчетов, из которых пользователь может начать изучение данных в верхней части иерархии и перейти к подробным сведениям. В этом учебнике было рассмотрено создание простого иерархического отчет, показывающий выбранной категории продуктов. Это осуществлялось с помощью элемента управления DropDownList для списка категорий и GridView для продуктов, принадлежащих выбранной категории.

В [следующее руководство](master-detail-filtering-with-two-dropdownlists-cs.md) перейти на один шаг интерфейс DropDownList в дальнейшем с помощью двумя элементами управления DropDownList.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Вперед](master-detail-filtering-with-two-dropdownlists-cs.md)
