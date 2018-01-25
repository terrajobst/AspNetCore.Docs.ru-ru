---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: "Добавление и реагирование на кнопок в элементе управления GridView (C#) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим способы добавления пользовательских кнопок в шаблон и полей элемента управления GridView и DetailsView. В частности мы будем bui..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f2a31f406bb1ed98e3620e216b4ad14fe59b32f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Добавление и реагирование на кнопок в элементе управления GridView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) или [скачать PDF](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> В этом учебнике мы рассмотрим способы добавления пользовательских кнопок в шаблон и полей элемента управления GridView и DetailsView. В частности мы создадим интерфейс, который имеет элемента, пользователь может просматривать поставщики страницы FormView.


## <a name="introduction"></a>Вступление

Хотя многих сценариях отчетов включают доступ только для чтения данных в отчете, не нередко для отчетов, которые включают следующие возможности для выполнения действий на основе отображаемых данных. Обычно это участвующих Добавление элемента управления Button, LinkButton или ImageButton Web с каждой записи отображаются в отчете, при нажатии вызывает обратную передачу и вызывает код для сервера. Изменение и удаление данных на основе записи по является наиболее распространенным. На самом деле, как мы видели, начиная с [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) так часто, что элементы управления GridView, DetailsView и FormView поддерживает такие функциональные возможности без учебник, редактирование и удаление требуется для написания ни единой строки кода.

Дополнительно на изменение и удаление кнопки GridView, DetailsView и FormView элементы управления могут также включать кнопок, элементов управления LinkButton и ImageButtons, при щелчке выполнять определенную настраиваемую логику на стороне сервера. В этом учебнике мы рассмотрим способы добавления пользовательских кнопок в шаблон и полей элемента управления GridView и DetailsView. В частности мы создадим интерфейс, который имеет элемента, пользователь может просматривать поставщики страницы FormView. Для данного поставщика FormView можно просмотреть сведения о поставщике вместе с веб-управления Button, если выбран вариант, будут отмечены все их соответствующих продуктов как неподдерживаемые. Кроме того, элемент управления GridView перечислены эти продукты, предоставляемые выбранного поставщика, с каждой строкой, содержащей увеличить цену и скидки цены кнопки, если нажата кнопка, вызывают или уменьшить продукта `UnitPrice` на 10% (см. рис. 1).


[![GridView и FormView содержит кнопки для выполнения настраиваемых действий](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Рис. 1**: оба FormView и GridView содержат кнопки, выполнить пользовательские действия ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Шаг 1: Добавление кнопки учебника веб-страницы

Прежде чем мы рассмотрим способы добавления пользовательских кнопок, сначала давайте немного времени для создания страниц ASP.NET в данном проекте веб-сайта, он понадобится для этого учебника. Начните с добавления в новую папку с именем `CustomButtons`. Добавьте следующие две страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `CustomButtons.aspx`


![Добавление страниц ASP.NET для пользовательских кнопок руководств](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**На рисунке 2**: Добавление страницы ASP.NET для пользовательских кнопок руководств


Как и в других папках, `Default.aspx` в `CustomButtons` папки будут перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Таким образом, добавьте этот пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений в режиме конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Рис. 3**: добавление `SectionLevelTutorialListing.ascx` пользовательский элемент управления `Default.aspx` ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Наконец, добавления страниц, как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после разбиения по страницам и сортировка `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элементы для редактирования, вставки и удаления учебники.


![Карта сайта теперь включает записи для пользовательских кнопок учебника](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Рис. 4**: карта сайта теперь включает записи для пользовательских кнопок учебника


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Шаг 2: Добавление FormView, в котором перечислены поставщики

Давайте начнем с учебником, добавив FormView со списком поставщиков. Как было описано во введении, это FormView позволит пользователю страницу с помощью поставщиков, продуктами, предоставляемый данным поставщиком в элементе управления GridView. Кроме того, этот FormView будет включить кнопку, при нажатии пометит все продукты поставщика как более не поддерживается. Перед сами относятся с добавлением пользовательской кнопки FormView, давайте сначала просто создать FormView, чтобы он отображал сведения о поставщиках.

Сначала откройте `CustomButtons.aspx` страницы в `CustomButtons` папки. Добавление страницы FormView, перетащив его из области элементов в область конструктора и задайте его `ID` свойства `Suppliers`. FormView смарт-теге, попробовать создать новый элемент управления ObjectDataSource с именем `SuppliersDataSource`.


[![Создать новый элемент управления ObjectDataSource SuppliersDataSource с именем](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Рис. 5**: создать новый именованный ObjectDataSource `SuppliersDataSource` ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Настроить этот новый элемент управления ObjectDataSource таким образом, что он запрашивает из `SuppliersBLL` класса `GetSuppliers()` метод (см. рис. 6). Поскольку это FormView не предоставляет интерфейс для обновления поставщика сведения, выберите вариант (нет) из раскрывающегося списка на вкладке «обновление».


[![Настройка источника данных с помощью класса SuppliersBLL s GetSuppliers()-метод](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Рис. 6**: Настройка источника данных для использования `SuppliersBLL` класса `GetSuppliers()` метод ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


После настройки ObjectDataSource, Visual Studio создаст `InsertItemTemplate`, `EditItemTemplate`, и `ItemTemplate` для FormView. Удалить `InsertItemTemplate` и `EditItemTemplate` и изменения `ItemTemplate` , чтобы он отображал просто поставщика компании имя и номер телефона. Наконец, включите поддержку разбиения на страницы FormView, установив флажок включения постраничного просмотра из его смарт-тега (или путем установки его `AllowPaging` свойства `True`). После внесения этих изменений на странице должна выглядеть следующим образом:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Рис. 7 показана страница CustomButtons.aspx при просмотре через браузер.


[![FormView перечислены CompanyName и поля телефона выбранного поставщика](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Рис. 7**: Перечисляет FormView `CompanyName` и `Phone` полей в настоящее время выбран поставщик ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Шаг 3: Добавление GridView, в которой перечислены продукты выбранного поставщика

Прежде чем добавить кнопку Прекратить все продукты FormView шаблона, давайте сначала добавить GridView под FormView, в которой перечислены продукты, предоставляемые выбранного поставщика. Чтобы выполнить это, добавьте на страницу GridView, задайте его `ID` свойства `SuppliersProducts`, и добавьте новый элемент управления ObjectDataSource с именем `SuppliersProductsDataSource`.


[![Создать новый элемент управления ObjectDataSource SuppliersProductsDataSource с именем](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Рис. 8**: создать новый именованный ObjectDataSource `SuppliersProductsDataSource` ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Настройте этот элемент управления ObjectDataSource использовать класс ProductsBLL `GetProductsBySupplierID(supplierID)` метод (см. рис. 9). Хотя этот GridView обеспечит цена продукта для настройки, не будет использоваться встроенные, изменение или удаление компонентов из GridView. Таким образом мы можно настроить раскрывающегося списка (нет) для ObjectDataSource, UPDATE, INSERT и удаление вкладок.


[![Настройка источника данных с помощью класса ProductsBLL s GetProductsBySupplierID(supplierID)-метод](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Рис. 9**: Настройка источника данных для использования `ProductsBLL` класса `GetProductsBySupplierID(supplierID)` метод ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Поскольку `GetProductsBySupplierID(supplierID)` метод принимает входной параметр, мастер ObjectDataSource запрашивает источник значения этого параметра. Для передачи в `SupplierID` значение из FormView, задайте параметр исходного раскрывающегося списка для элемента управления, а также список раскрывающегося списка ControlID `Suppliers` (идентификатор FormView созданный на шаге 2).


[![Указывает, что supplierID параметр должен предоставляться управления FormView поставщиков](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Рис. 10**: указать, что  *`supplierID`*  параметр должен предоставляться `Suppliers` управления FormView ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


После завершения работы мастера ObjectDataSource GridView будет содержать BoundField или CheckBoxField для каждого из полей данных продукта. Давайте урезать это для отображения только `ProductName` и `UnitPrice` стояли вместе с `Discontinued` CheckBoxField; Кроме того, давайте форматирования `UnitPrice` BoundField таким образом, что его текст форматируется в виде валюты. К GridView и `SuppliersProductsDataSource` ObjectDataSource должна выглядеть примерно следующим образом:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

На этом этапе в этом руководстве отображает сведения об образце отчета, позволяя пользователю выбрать поставщика из FormView вверху и просмотреть продукты, предоставляемый данным поставщиком через GridView в нижней. Рис. 11 показана снимок экрана этой страницы при выборе поставщика Tokyo Traders из FormView.


[![Продукты s выбранного поставщика отображаются в GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Рис. 11**: выбранного поставщика продукты отображаются в GridView ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Шаг 4: Создание DAL и уровень бизнес-ЛОГИКИ методов прекратить все продукты для поставщика

Прежде чем мы можем добавить кнопку к FormView, при щелчке прекращает всех продуктов поставщика, необходимо сначала добавить метод DAL и уровень бизнес-ЛОГИКИ, выполняющий это действие. В частности, этот метод будет называться `DiscontinueAllProductsForSupplier(supplierID)`. При нажатии кнопки FormView, мы будем вызывать этот метод в уровня бизнес-логики, передача в выбранный поставщик `SupplierID`; МЕТОДА затем будет обращаться к метод соответствующий уровень доступа к данным, который будет выдавать `UPDATE` инструкции к базе данных, прекращает продукты указанного поставщика.

Как уже были выполнены в предыдущих учебных, мы будем использовать подход снизу вверх, начиная с созданием метод DAL, затем метод уровень бизнес-ЛОГИКИ и наконец реализации функциональности на странице ASP.NET. Откройте `Northwind.xsd` типизированного набора данных в `App_Code/DAL` папки и добавьте новый метод `ProductsTableAdapter` (щелкните правой кнопкой мыши `ProductsTableAdapter` и выберите Добавить запрос). При этом откроется мастер настройки запроса адаптера таблицы, который нам пошаговое описание процесса добавления нового метода. Запустите, указывая, что наши DAL метод будет использовать нерегламентированные инструкции SQL.


[![Создание метода DAL, с помощью инструкции SQL Ad-Hoc](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Рис. 12**: создать метод DAL с помощью инструкции SQL Ad-Hoc ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


Затем мастер предлагает нам о том, какой тип создаваемого запроса. Поскольку `DiscontinueAllProductsForSupplier(supplierID)` метод будет необходимо обновить `Products` таблице базы данных, задание `Discontinued` на 1 для всех продуктов, представленной указанным  *`supplierID`* , нам нужно создать запрос, который обновляет данные.


[![Выберите тип запроса UPDATE](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Рис. 13**: выберите тип запроса UPDATE ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


На следующем экране мастера предоставляет текущих TableAdapter `UPDATE` инструкцию, которая обновляет каждого из полей, определенных в `Products` DataTable. Замените этот текст запроса с помощью следующей инструкции:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

После ввода этого запроса и нажмите кнопку Далее, последний экран мастера запрашивает используйте имя нового метода `DiscontinueAllProductsForSupplier`. Завершите работу мастера, нажав кнопку "Готово". При возвращении в конструкторе наборов данных вы увидите новый метод в `ProductsTableAdapter` с именем `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Имя нового метода DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Рис. 14**: имя нового метода DAL `DiscontinueAllProductsForSupplier` ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


С `DiscontinueAllProductsForSupplier(supplierID)` метод, созданный на уровне доступа к данным, теперь нам требуется создать `DiscontinueAllProductsForSupplier(supplierID)` метод в бизнес-логики. Для этого откройте `ProductsBLL` и добавьте следующее:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Этот метод просто вызывает вплоть до `DiscontinueAllProductsForSupplier(supplierID)` метод DAL, передавая указанных  *`supplierID`*  значение параметра. Если бы все бизнес-правила, которые допускается только продукты поставщика прерывание при определенных обстоятельствах, эти правила должны быть реализованы здесь в МЕТОДА.

> [!NOTE]
> В отличие от `UpdateProduct` перегрузки в `ProductsBLL` класса `DiscontinueAllProductsForSupplier(supplierID)` сигнатура метода не содержит `DataObjectMethodAttribute` атрибут (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Это исключает `DiscontinueAllProductsForSupplier(supplierID)` метода из мастера настройки источника данных ObjectDataSource раскрывающегося списка на вкладке «обновление». Я хранить этот атрибут опущен, так как мы будем вызов `DiscontinueAllProductsForSupplier(supplierID)` метод непосредственно из обработчика событий в нашу страницу ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Шаг 5: Добавление прекратить кнопку все продукты в FormView

С `DiscontinueAllProductsForSupplier(supplierID)` метод в уровень бизнес-ЛОГИКИ и завершения DAL последний шаг по добавлению возможность прекратить все продукты для выбранного поставщика — добавить кнопку веб-элемент управления FormView `ItemTemplate`. Давайте добавим кнопки ниже номер телефона поставщика с текстом кнопки, прекратите все продукты и `ID` значение свойства `DiscontinueAllProductsForSupplier`. Это кнопка веб-элемента управления с помощью конструктора можно добавить, щелкнув ссылку "Правка шаблонов" в FormView смарт-тег (см. рис. 15), или напрямую с помощью декларативного синтаксиса.


[![Добавить прекратить все продукты кнопку веб-элемента управления FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Рис. 15**: Добавление прекратить все продукты кнопку веб-элемент управления FormView `ItemTemplate` ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


При нажатии кнопки, посетив пользователя, затем следует странице обратной передачи и FormView [ `ItemCommand` событие](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) активируется. Для выполнения пользовательского кода в ответ на эту кнопку, выбираемой, можно создать обработчик событий для этого события. Понять, однако, `ItemCommand` событие вызывается каждый раз, когда *любой* щелчке элемента управления Button, LinkButton или ImageButton Web в FormView. Это означает, что при перемещении с одной страницы на другую в FormView `ItemCommand` события; одинаково, когда пользователь нажимает кнопку Создать, изменить, или удалить в FormView, который поддерживает вставку, обновление или удаление.

Поскольку `ItemCommand` возникает независимо от того, какая кнопка нажата в случае, когда обработчик мы должны определять, если отказаться от всех продуктов была нажата кнопка или если он был некоторые другие кнопки. Чтобы сделать это, можно задать кнопку веб-элемента управления `CommandName` некоторые отличительные значение свойства. При нажатии кнопки, это `CommandName` значение передается в `ItemCommand` обработчика событий, позволяющие определить, успешно ли кнопку Прекратить все продукты кнопки, нажатой. Задать прекратить все продукты кнопки `CommandName` свойства DiscontinueProducts.

Наконец воспользуемся диалоговое окно подтверждения на стороне клиента для убедитесь, что пользователь действительно требуется прекратить продуктов выбранного поставщика. Как мы видели в [Добавление подтверждения клиентских при удалении](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) учебника, это можно сделать с помощью небольшой JavaScript. В частности значение свойства OnClientClick кнопку веб-элемента управления`return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

После внесения этих изменений, FormView должен выглядеть следующим образом:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Создайте обработчик событий для FormView `ItemCommand` событий. В этом обработчике событий, необходимо сначала определить, является ли прекратить все продукты была нажата кнопка. Если Да, мы хотим создать экземпляр `ProductsBLL` класса и вызвать его `DiscontinueAllProductsForSupplier(supplierID)` метод, передавая `SupplierID` из выбранного FormView:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Обратите внимание, что `SupplierID` текущего выбранного поставщика в FormView возможен доступ с помощью FormView [ `SelectedValue` свойства](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Свойство возвращает значение для записи, отображаемых в FormView ключа первых данных. FormView [ `DataKeyNames` свойство](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), указывающая данные полей, из которых данные ключевые значения извлекаются из, автоматически присвоено `SupplierID` средой Visual Studio, когда привязка ObjectDataSource FormView в шаге 2.

С `ItemCommand` обработчик событий создан, теперь пора проверить страницу. Перейдите к Cooperativa de Quesos поставщика «Las Cabras» (Это пятая поставщика в FormView для меня). Этот поставщик предоставляет два продукта, Queso Cabrales и Queso Manchego La Pastora, которые являются *не* более не поддерживается.

Предположим, что Cooperativa de Quesos «Las Cabras» вышло за пределы предприятия и, следовательно, его продуктов прерывание. Нажмите кнопку Прекратить кнопку все продукты. Появится диалоговое окно клиентского подтверждения (см. рис. 16).


[![Cooperativa de Quesos Las Cabras предоставляет две активные продукты.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**На рисунке 16**: Cooperativa de Quesos Las Cabras предоставляет два Active продукта ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Если нажать кнопку ОК в диалоговом окне подтверждение на стороне клиента, отправки формы будет продолжена, вызывает обратную передачу, в котором FormView `ItemCommand` событие будет срабатывать. Мы создали обработчик события будет выполняться, вызов `DiscontinueAllProductsForSupplier(supplierID)` метод и прекращение Queso Cabrales и Queso Manchego La Pastora продуктов.

При отключении состояния представления GridView GridView повторно привязываются в базовом хранилище данных для каждой обратной передачи и поэтому немедленно обновляется для отражения того, что эти два продукта теперь не поддерживаются (см. Рисунок 17). Если, однако вы не отключили состояние просмотра в GridView, необходимо будет вручную изменить привязку данных к GridView после внесения этого изменения. Чтобы сделать это, просто выполнить вызов к GridView `DataBind()` метод сразу после вызова `DiscontinueAllProductsForSupplier(supplierID)` метод.


[![После нажатия кнопки Прекратить все продукты, продукты, поставщик s — обновлен соответствующим образом](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Рисунок 17**: после нажатия кнопки Прекратить все продукты поставщиков продуктов, которые обновлены соответствующим образом ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Шаг 6: Создание UpdateProduct перегрузку в бизнес-логики для настройки цена продукта

Как прекратить все продукты кнопкой в FormView, чтобы добавить кнопки для увеличения и уменьшения цену для продукта в GridView необходимо сначала добавить соответствующие методы доступа к данным и бизнес-логики. Так как мы уже есть метод, который обновляет строку один продукт в DAL, мы предоставляем такие функциональные возможности, создав новый перегруженный `UpdateProduct` метод в МЕТОДА.

Наши последние `UpdateProduct` перегрузки предпримете как скалярных входных значений любого их сочетания полей продукта и затем обновлены только те поля, для указанного продукта. Для этой перегрузки будет немного отличаться от этого стандарта и вместо передачи в пределах продукта `ProductID` и процент, при котором `UnitPrice` (в отличие от передачи в новом скорректированы `UnitPrice` сам). Такой подход будет упростить код, нам нужно записать в класс кода страницы ASP.NET, так как мы не должны использоваться в определении текущего продукта `UnitPrice`.

`UpdateProduct` Перегрузки для этого учебника, показано ниже:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Эта перегрузка получает сведения об указанной продукта через DAL `GetProductByProductID(productID)` метод. Затем проверяет, является ли продукта `UnitPrice` назначается базы данных `NULL` значение. Если это так, цена остается без изменений. Если, однако имеется значение, отличное от`NULL` `UnitPrice` значение, метод обновляет продукта `UnitPrice` указанного процентов (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Шаг 7: Добавление увеличение и уменьшение кнопок в GridView

GridView (и DetailsView) оба состоят из коллекции полей. В дополнение к стояли CheckBoxFields и TemplateFields ASP.NET включает ButtonField, который, как и предполагает его имя, отображается в виде столбца с кнопкой, LinkButton или ImageButton для каждой строки. Аналогично FormView, щелкнув *любой* кнопки в GridView кнопки разбиения по страницам, изменить или удалить кнопки, кнопки сортировки и т. д обратную передачу, вызывает GridView [ `RowCommand` событие](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Имеет ButtonField `CommandName` свойство, которое присваивает указанное значение для каждого из ее кнопок `CommandName` свойства. Как и с FormView `CommandName` значение используется `RowCommand` обработчик событий, чтобы определить, какая кнопка была нажата.

Давайте добавим два новых ButtonFields GridView, одним текстом кнопки Price + 10%, а другой с текстом прайс -10%. Чтобы добавить эти ButtonFields, щелкните ссылку Правка столбцов GridView смарт-теге, выберите тип ButtonField поля из списка в верхнем левом углу и нажмите кнопку Добавить.


![Добавьте два ButtonFields GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**На рисунке 18**: добавление двух ButtonFields GridView


Переместите два ButtonFields, чтобы они отображались как первые два поля GridView. Затем задайте `Text` свойства этих двух ButtonFields цену + 10% и цена -10% и `CommandName` свойства IncreasePrice и DecreasePrice, соответственно. По умолчанию ButtonField подготавливает его столбец кнопок элементов управления LinkButton. Это можно изменить, однако через ButtonField [ `ButtonType` свойства](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Давайте у этих двух ButtonFields к просмотру в виде обычных кнопок; Таким образом, задать `ButtonType` свойства `Button`. На рисунке 19 диалоговое окно показывает поля, после внесения этих изменений; Далее приведен декларативная разметка GridView.


![Настройка свойств ButtonType, CommandName и ButtonFields текста](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**На рисунке 19**: Настройка ButtonFields `Text`, `CommandName`, и `ButtonType` свойства


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

С этих ButtonFields создан, последний шаг является создание обработчика событий для элемента GridView `RowCommand` событий. Этот обработчик событий, если сработала из-за либо цены + 10% или % кнопок прайс -10 щелкают, необходимо определить `ProductID` для строки, кнопка была нажата, а затем вызвать `ProductsBLL` класса `UpdateProduct` метод, передавая соответствующие `UnitPrice` Коррекция процента вместе с `ProductID`. Следующий код выполняет следующие задачи:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Чтобы определить `ProductID` для строки, цена которых + 10% или прайс -10% кнопка была нажата, то необходимо обратиться к GridView `DataKeys` коллекции. Эта коллекция содержит значения полей, указанные в `DataKeyNames` свойства для каждой строки в GridView. С момента GridView `DataKeyNames` свойство имеет значение ProductID по Visual Studio при привязке к GridView ObjectDataSource `DataKeys(rowIndex).Value` предоставляет `ProductID` для указанного *rowIndex*.

В автоматически передает ButtonField *rowIndex* строки, для которого была нажата через `e.CommandArgument` параметра. Таким образом чтобы определить `ProductID` для строки, цена которых + 10% или прайс -10% кнопка была нажата, мы используем: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

С помощью кнопки Прекратить все продукты, состояние представления элемента GridView, после отключения GridView повторно привязываются в базовом хранилище данных для каждой обратной передачи, а таким образом немедленно обновляется для отражения изменений цен, возникающее в результате щелкнув любую из кнопок. Если, однако вы не отключили состояние просмотра в GridView, необходимо будет вручную изменить привязку данных к GridView после внесения этого изменения. Чтобы сделать это, просто выполнить вызов к GridView `DataBind()` метод сразу после вызова `UpdateProduct` метод.

Рис. 20 показана страница при просмотре продуктов, предоставляемые бабушка Келли Homestead. Рисунок 21 демонстрирует результаты после Price + 10% была нажата кнопка дважды для распределения Ежевичный Ягодное и кнопка прайс -10% один раз для Sauce клюквенный Норвудский.


[![GridView включает цену + 10% и % кнопок прайс -10](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Рис. 20**: цены включает GridView + 10% и % кнопок прайс -10 ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Цены для первой и третьей продукта были обновлены через Price + 10% и % кнопок прайс -10](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Рисунок 21**: цены для первого и третьего продукта были обновлены через Price + 10% и % кнопок прайс -10 ([Просмотр полноразмерное изображение](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> Кнопок, элементов управления LinkButton и добавить их TemplateFields ImageButtons GridView (и DetailsView) может иметь. Как с BoundField, при нажатии этих кнопок будет вызывать обратную передачу, возведение в GridView `RowCommand` событий. При добавлении кнопок в TemplateField, однако кнопки `CommandArgument` не задается автоматически индекс строки, как если бы использование ButtonFields. Если необходимо определить индекс строки кнопки, которая была нажата в течение `RowCommand` обработчика событий, необходимо вручную задать свойству кнопки `CommandArgument` свойства его декларативный синтаксис в TemplateField, с помощью следующего кода:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.


## <a name="summary"></a>Сводка

GridView, DetailsView и FormView во всех элементах управления могут включать кнопок, элементов управления LinkButton и ImageButtons. Такие кнопки при щелчке вызывает обратную передачу и вызвать `ItemCommand` события в элементах управления FormView и DetailsView и `RowCommand` событий в GridView. Эти данные веб-элементов управления имеют встроенные функциональные возможности для обработки таких действий, связанных с командой, как удаление или изменение записей. Тем не менее, мы также можно использовать кнопки, при нажатии, ответ с выполнением собственный пользовательский код.

Для этого необходимо создать обработчик событий для `ItemCommand` или `RowCommand` событий. В этот обработчик событий сначала проверяется входящего `CommandName` значение, чтобы определить, какая кнопка была нажата и затем предпринять соответствующее действие пользовательского. В этом учебнике мы узнали, как использовать кнопки и ButtonFields прекратить все продукты для указанного поставщика, или чтобы увеличить или уменьшить цену конкретного продукта на 10%.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Вперед](adding-and-responding-to-buttons-to-a-gridview-vb.md)
