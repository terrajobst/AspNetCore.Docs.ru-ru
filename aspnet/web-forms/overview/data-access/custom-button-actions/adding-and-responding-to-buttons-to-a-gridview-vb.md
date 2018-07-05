---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Добавление и реагирование на кнопки к GridView (VB) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы рассмотрим добавление новых кнопок к шаблону и полям элемента управления GridView или DetailsView. В частности мы будем Командная строка построения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 458f90bf70d6f10402583623ef62bf1572040ce4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365062"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Добавление и реагирование на кнопки к GridView (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) или [скачать PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> В этом руководстве мы рассмотрим добавление новых кнопок к шаблону и полям элемента управления GridView или DetailsView. В частности мы создадим интерфейс, имеющий FormView, который позволяет пользователю просматривать поставщиков.


## <a name="introduction"></a>Вступление

Хотя многие сценарии создания отчетов включают в себя доступ только для чтения данных в отчете, его s не часто, в отчетах включить возможность выполнять действия на основе данных, которые отображаются. Обычно это участвует, добавление элемента управления Button, LinkButton или ImageButton Web с каждой записи, отображенной в отчете, при нажатии вызывает обратную передачу и вызывает код на стороне сервера. Редактирование и удаление данных на основе записи по — это наиболее распространенный пример. На самом деле, как мы видели, начиная с [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) учебник, редактирование и удаление так часто, что элементы управления GridView, DetailsView и FormView могут поддерживать подобные функции без необходимые для написания кода.

Кроме того на изменение и удаление кнопки, GridView, DetailsView и FormView элементы управления могут также содержать кнопок, элементов управления LinkButton или ImageButtons, при нажатии выполняют определенную настраиваемую логику на стороне сервера. В этом руководстве мы рассмотрим добавление новых кнопок к шаблону и полям элемента управления GridView или DetailsView. В частности мы создадим интерфейс, имеющий FormView, который позволяет пользователю просматривать поставщиков. Для определенного поставщика FormView показывают сведения о поставщике, а также кнопку веб-элемент управления, при нажатии пометит все связанные с ним продукты как снятые с продажи. Кроме того, элемент управления GridView перечисляются продукты, предоставляемые выбранным поставщиком, где каждая строка, содержащая увеличить цену и скидки цена кнопок, если выбран этот вариант, повысить или снизить продукт s `UnitPrice` на 10% (см. рис. 1).


[![FormView и GridView содержат кнопки, выполняющие настраиваемые действия](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Рис. 1**: оба FormView и GridView содержат кнопки, выполнить пользовательские действия ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Шаг 1: Добавление веб-страниц учебного кнопки

Прежде чем мы рассмотрим, как добавление новых кнопок, позвольте s уделим несколько минут созданию страниц ASP.NET в нашем проекте веб-сайта, что нам понадобится для этого руководства. Начните с добавления новой папки с именем `CustomButtons`. Добавьте следующие две страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `CustomButtons.aspx`


![Добавление страниц ASP.NET для пользовательских кнопок руководств](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Рис. 2**: Добавление страниц ASP.NET для пользовательских кнопок руководств


Как и в других папках, `Default.aspx` в `CustomButtons` папку перечислит учебные курсы в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Поэтому добавьте данный пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s режиме конструктора.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Рис. 3**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Наконец, добавьте страницы как записи, чтобы `Web.sitemap` файл. В частности, добавьте следующую разметку после разбиения по страницам и сортировка `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элементы для редактирования, вставки и удаления учебных курсов.


![Карта узла теперь есть запись для пользовательских кнопок учебника](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Рис. 4**: карта узла теперь есть запись для пользовательских кнопок учебника


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Шаг 2: Добавление элемента FormView, перечисляющего поставщиков

Позвольте s начало работы с учебником, добавив FormView, перечисляющего поставщиков. Как описано во введении, этот FormView позволит пользователю просматривать поставщиков, показывая продукты, предоставляемые поставщиком, в элементе управления GridView. Кроме того, этот FormView будет включать кнопки, при нажатии пометит все продукты поставщика s как снятые с продажи. Прежде чем заняться добавлением специальной кнопки к элементу FormView, позвольте s сперва создадим элемент FormView, чтобы он отображал сведений о поставщике.

Сначала откройте `CustomButtons.aspx` странице в `CustomButtons` папку. Добавление элемента FormView на страницу, перетащив его с панели инструментов в конструктор и задайте его `ID` свойства `Suppliers`. Из s смарт-тега FormView, необязательно, чтобы создать новый ObjectDataSource, именуемый `SuppliersDataSource`.


[![Создайте новый ObjectDataSource, именуемого SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Рис. 5**: создайте новый ObjectDataSource с именем `SuppliersDataSource` ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Настроить этот новый элемент управления ObjectDataSource, таким образом, чтобы он запрашивал относящийся к `SuppliersBLL` класс s `GetSuppliers()` метод (см. рис. 6). Так как этот FormView не предоставляет интерфейс для обновления сведений о поставщике, выберите что параметр (None) из раскрывающегося списка на вкладке "обновления".


[![Настройка источника данных для использования метода Getsuppliers() класса s метода GetSuppliers()](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Рис. 6**: Настройка источника данных для использования `SuppliersBLL` класс s `GetSuppliers()` метод ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


После настройки ObjectDataSource, Visual Studio создаст `InsertItemTemplate`, `EditItemTemplate`, и `ItemTemplate` для FormView. Удалить `InsertItemTemplate` и `EditItemTemplate` и изменение `ItemTemplate` так, чтобы он отображался только поставщик s компании имя и номер телефона. Наконец, включите поддержку разбиения по страницам для FormView, установив флажок Включить разбиение по страницам из его смарт-тега (или установив его `AllowPaging` свойства `True`). После внесения этих изменений декларативная разметка s страницы должен выглядеть следующим образом:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Рис. 7 показана страница CustomButtons.aspx, просматриваемая в обозревателе.


[![FormView отображает поля CompanyName и Phone от выбранного поставщика](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Рис. 7**: The FormView отображает `CompanyName` и `Phone` поля из в настоящее время выбран поставщик ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Шаг 3: Добавление GridView, которая перечисляет продукты выбранного поставщика s

Перед добавлением в FormView s шаблон «снять с продажи все продукты», позвольте s сначала добавить GridView под элемента FormView, перечисляющего продукты, предоставляемые выбранным поставщиком. Чтобы выполнить это, добавить GridView к странице, задайте его `ID` свойства `SuppliersProducts`, и добавьте новый ObjectDataSource, именуемый `SuppliersProductsDataSource`.


[![Создайте новый ObjectDataSource, именуемый SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Рис. 8**: создайте новый ObjectDataSource с именем `SuppliersProductsDataSource` ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Настроить данный элемент управления ObjectDataSource на использование класса ProductsBLL s `GetProductsBySupplierID(supplierID)` метод (см. рис. 9). При этом GridView позволит корректировать цену продукта s, он победил t будет использовать встроенные возможности правки или удаления из GridView. Таким образом можно задать стрелку раскрывающегося списка (нет) для ObjectDataSource s вкладках UPDATE, INSERT и DELETE.


[![Настройка источника данных для использования класса ProductsBLL s GetProductsBySupplierID(supplierID)-метод](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Рис. 9**: Настройка источника данных для использования `ProductsBLL` класс s `GetProductsBySupplierID(supplierID)` метод ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Так как `GetProductsBySupplierID(supplierID)` метод принимает входной параметр, мастер ObjectDataSource запрашивает для источника значение этого параметра. Для передачи в `SupplierID` из FormView, необходимо установить параметр исходного стрелку раскрывающегося списка элемента управления и выберите в раскрывающемся списке ControlID, чтобы `Suppliers` (идентификатор FormView, созданного на шаге 2).


[![Указывает, что столбец supplierID должен исходить от элемента управления Suppliers FormView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Рис. 10**: Указывает, что *`supplierID`* должен исходить от `Suppliers` управления FormView ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


После завершения работы мастера ObjectDataSource GridView будет содержать BoundField или CheckBoxField для каждого из полей данных продукта s. Let s сократим это, чтобы показать только `ProductName` и `UnitPrice` поля BoundField, кроме вместе с `Discontinued` CheckBoxField; Кроме того, позвольте формата s `UnitPrice` BoundField таким образом, чтобы его текст форматировался как денежная единица. В GridView и `SuppliersProductsDataSource` декларативная разметка ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

На этом этапе наш учебный курс отображает отчет основной/подробности, позволяя пользователю выбрать поставщика из FormView наверху и просмотреть продукты, предоставляемые поставщиком, через GridView в нижней. Рис. 11 показан снимок экрана этой страницы при выборе поставщика Tokyo Traders из FormView.


[![Продукты выбранного поставщика, отображаются в GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Рис. 11**: выбран Supplier s продукты отображаются в GridView ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Шаг 4: Создание DAL и BLL методы, чтобы снять с продажи все продукты поставщика

Прежде чем мы можем добавить кнопку к элементу FormView, при щелчке, прекращает все продукты поставщика s, сначала необходимо добавить метод DAL и BLL, который выполняет это действие. В частности, этот метод будет называться `DiscontinueAllProductsForSupplier(supplierID)`. При нажатии кнопки преобразования FormView мы этот метод вызывается в уровне бизнес-логики, передав в выбранном поставщике s `SupplierID`; BLL затем вызовет соответствующий метод уровня доступа к данным, который будет выдавать `UPDATE` инструкцию, чтобы База данных, прекращает продукты указанного поставщика s.

Как было показано в предыдущих учебных курсах, мы используем подход снизу вверх, начиная с создания метода DAL, затем метода BLL и наконец реализуя функции страницы ASP.NET. Откройте `Northwind.xsd` типизированного набора DataSet в `App_Code/DAL` папку и добавьте новый метод в `ProductsTableAdapter` (щелкните правой кнопкой мыши `ProductsTableAdapter` и выберите Добавить запрос). Это приведет к появлению мастер настройки запроса TableAdapter, который проведет пользователя через процесс добавления нового метода. Для начала, указывающее, что наш метод DAL будет использовать специальный оператор SQL.


[![Создание метода DAL, с помощью инструкции SQL Ad-Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Рис. 12**: Создание метода DAL с использованием инструкции SQL Ad-Hoc ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


После этого мастер предложит тип создаваемого запроса. Так как `DiscontinueAllProductsForSupplier(supplierID)` будет необходимо обновить метод `Products` таблице базы данных, задание `Discontinued` на 1 для всех продуктов с определенным идентификатором *`supplierID`*, нам нужно создать запрос, обновляющий данные.


[![Выберите тип запроса UPDATE](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Рис. 13**: выберите тип запроса UPDATE ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


На следующем экране мастера предоставляет существующий адаптер таблицы s `UPDATE` оператор, который обновляет каждое из полей, определенных в `Products` DataTable. Замените этот текст запроса с помощью следующей инструкции:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

После ввода этого запроса и нажмите кнопку Далее, на последнем экране запросит имя нового метода s использовать `DiscontinueAllProductsForSupplier`. Следуйте указаниям мастера, нажав кнопку "Готово". При возвращении в конструктор DataSet, вы должны увидеть новый метод в `ProductsTableAdapter` с именем `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Имя нового метода DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Рис. 14**: новому методу DAL имя `DiscontinueAllProductsForSupplier` ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


С помощью `DiscontinueAllProductsForSupplier(supplierID)` метод, созданный на уровень доступа к данным, DAL следующей задачей является создание `DiscontinueAllProductsForSupplier(supplierID)` метод на уровне бизнес-логики. Для этого откройте `ProductsBLL` и добавьте следующее:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Этот метод просто вызывает метод `DiscontinueAllProductsForSupplier(supplierID)` в DAL, передавая предоставленное *`supplierID`* значение параметра. Если бизнес-правила, которые разрешается использовать только поставщик продукты прерывание при определенных обстоятельствах, эти правила следует применить здесь, в BLL.

> [!NOTE]
> В отличие от `UpdateProduct` перегрузки в `ProductsBLL` класс, `DiscontinueAllProductsForSupplier(supplierID)` сигнатура метода не включает `DataObjectMethodAttribute` атрибут (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Это исключает возможность `DiscontinueAllProductsForSupplier(supplierID)` метода из раскрывающегося списка ObjectDataSource s s мастер настройки источников данных на вкладке "обновления". Я ve пропустил этот атрибут, так как мы будем вызывать `DiscontinueAllProductsForSupplier(supplierID)` метод напрямую из обработчика событий на нашей странице ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Шаг 5: Добавление снять с продажи все продукты кнопки к элементу FormView

С помощью `DiscontinueAllProductsForSupplier(supplierID)` выполнить метод в BLL и DAL, Заключительным этапом добавления возможности снять с продажи все продукты для выбранного поставщика является добавление кнопки веб-элемент управления FormView s `ItemTemplate`. S позволяют добавить кнопки ниже номер телефона поставщика s с текстом кнопки, снять с продажи все продукты и `ID` значение свойства `DiscontinueAllProductsForSupplier`. Это кнопка веб-элемента управления в конструкторе можно добавить, щелкнув ссылку Изменить шаблоны в смарт-тега FormView s (см. рис. 15), или напрямую через декларативный синтаксис.


[![Добавление снять с продажи все продукты кнопку веб-элемента управления FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Рис. 15**: добавить снять с продажи все продукты кнопку веб-элемент управления FormView s `ItemTemplate` ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


При нажатии кнопки, посетив пользователя, страницы следует обратная передача и FormView s [ `ItemCommand` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) активируется. Для выполнения пользовательского кода в ответ на эту кнопку, выбираемой, создадим обработчик событий для данного события. Понимать, что `ItemCommand` событие запускается каждый раз, когда *любой* кнопки LinkButton и ImageButton Web внутри FormView. Это означает, что при перемещении с одной страницы на другую в FormView, `ItemCommand` вызывает событие, то же, когда пользователь нажимает кнопку "Создать", "Правка, удаление в элементе управления FormView, поддерживающего вставки, обновления или удаления.

Так как `ItemCommand` срабатывает независимо от того, какая кнопка нажата, в случае обработчик, необходим способ для определения того, если снять с продажи все продукты была нажата кнопка или если он был какая-либо другая. Чтобы добиться этого, можно задать кнопку веб-элементе управления s `CommandName` некоторые идентифицирующие значение свойства. При нажатии кнопки, это `CommandName` значение, передаваемое в `ItemCommand` обработчик событий, благодаря чему мы можем определить, была ли «снять с продажи все продукты» кнопки, нажатой. Набор s снять с продажи все продукты кнопку `CommandName` свойства DiscontinueProducts.

Наконец позволяют использовать диалоговое окно подтверждения на стороне клиента, чтобы убедиться, что пользователь действительно желает снять с продажи продукты выбранного поставщика s s. Как мы видели в [Добавление клиентского подтверждения при Идет удаление](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) учебника, это можно сделать с помощью немного кода JavaScript. В частности значение свойства OnClientClick s Web кнопки элемента управления `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

После внесения этих изменений декларативный синтаксис s FormView должна выглядеть следующим образом:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Создайте обработчик событий для FormView s `ItemCommand` событий. В этом обработчике событий, необходимо сначала определить, была ли нажата кнопка снять с продажи все продукты. Если таким образом, мы хотим создать экземпляр `ProductsBLL` и вызовите его `DiscontinueAllProductsForSupplier(supplierID)` метод, передавая `SupplierID` из выбранного FormView:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Обратите внимание, что `SupplierID` текущего выбранного поставщика в FormView может осуществляться с использованием FormView s [ `SelectedValue` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Свойство возвращает значение первого ключа данных для записи, отображаемой в FormView. FormView s [ `DataKeyNames` свойство](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), который указывает на данные, извлеченные из, поля, из которых значения ключа данных было автоматически установлено на `SupplierID` средой Visual Studio при привязке элемента управления ObjectDataSource на задний план FormView на шаге 2.

С помощью `ItemCommand` обработчик событий создан, пора протестировать страницу. Перейдите к Cooperativa de Quesos 'Las Cabras' поставщика (он s пятый поставщик в FormView для меня). Этот поставщик поставляет два продукта, Queso Cabrales и Queso Manchego La Pastora, оба из которых являются *не* более не поддерживается.

Представьте себе, что Cooperativa de Quesos 'Las Cabras' вышла из бизнеса и, следовательно, его продукты прерывание. Нажмите кнопку Снять с продажи все продукты кнопки. Откроется диалоговое окно подтверждения на стороне клиента (см. рис. 16).


[![Cooperativa de Quesos Las Cabras предоставляет два активных продукта](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Рис. 16**: Cooperativa de Quesos Las Cabras предоставляет два активных продукта ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Если вы щелкнете OK в диалоговом окне подтверждения на стороне клиента, последует Отправка формы, вызывая обратную передачу, в котором FormView s `ItemCommand` событие будет срабатывать. Созданный обработчик событий выполнится, вызывая `DiscontinueAllProductsForSupplier(supplierID)` метод и прекращение поддержки продукты Queso Cabrales и Queso Manchego La Pastora.

При отключении состояния представления GridView s GridView лежащему в базовом хранилище данных при каждой обратной передаче, а следовательно, будет немедленно обновлен для отражения, что эти два продукта производства (см. рис. 17). Если, однако вы не отключили состояние представления в GridView, необходимо будет вручную повторную привязку данных к GridView после внесения этого изменения. Чтобы выполнить это, просто отправьте вызов в GridView s `DataBind()` сразу же после вызова `DiscontinueAllProductsForSupplier(supplierID)` метод.


[![После нажатия кнопки «снять с продажи все продукты», поставщик s они обновлены соответствующим образом](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Рис. 17**: после нажатия кнопки «снять с продажи все продукты», поставщик s они обновлены соответствующим образом ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Шаг 6: Создание перегрузки UpdateProduct в уровне бизнес-логики для корректировки цены продукта s

Как и с «снять с продажи все продукты» в FormView, чтобы добавить кнопки для повышения и понижения цены продукта в GridView нам нужно сначала добавить соответствующие методы уровня доступа к данным и бизнес-логики. Поскольку у нас уже есть метод, который обновляет одну строку продукта DAL, можно предоставить подобные функции, создав новую перегрузку для `UpdateProduct` в BLL.

Наши предыдущие перегрузки `UpdateProduct` перегрузки предприняли ряд сочетаний полей продукта как скалярные входные значения и затем обновляли только эти поля для указанного продукта. Для этой перегрузки мы немного отличаются от этого стандарта и вместо этого передайте в продукте s `ProductID` и процент, на который следует изменить `UnitPrice` (в отличие от передачи в новом корректируется `UnitPrice` самого). Такой подход упростит код, нам нужно записать в классе фонового кода страницы ASP.NET, так как мы кое t нужно затруднять себя определением для текущего продукта s `UnitPrice`.

`UpdateProduct` Перегрузки для данного учебного курса приведена ниже:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Перегрузка извлекает информацию об указанном продукте через метод DAL s `GetProductByProductID(productID)` метод. Затем она проверяет см. в разделе ли продукт s `UnitPrice` продукта значение `NULL` значение. Если это так, цена остается без изменений. Если, однако, отличный от`NULL` `UnitPrice` значение, метод обновляет продукт s `UnitPrice` указанного процентов (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Шаг 7: Добавление кнопок увеличения и уменьшения к элементу GridView

GridView (и DetailsView) состоят из собраний полей. Помимо полей BoundField CheckBoxFields и поля TemplateField ASP.NET включает в себя ButtonField, который, как и предполагает его имя, отображается в виде столбца с помощью кнопки, LinkButton или ImageButton для каждой строки. Аналогичную FormView, щелкнув *любой* кнопки в GridView кнопки перелистывания, изменить или удалить кнопки, кнопок упорядочения и т. д., вызывает обратную передачу и инициирует GridView s [ `RowCommand` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Имеет ButtonField `CommandName` свойство, которое назначает указанное значение всем свойствам `CommandName` свойства. Как с помощью FormView, `CommandName` значение используется `RowCommand` обработчик событий, чтобы определить, какая кнопка была нажата.

Let s добавление двух новых полей Buttonfield к GridView, одно с текстом кнопки цена + 10%, а другой с текстом цена -10%. Добавление этих полей Buttonfield, щелкните ссылку Изменить столбцы в смарт-теге GridView s, выберите типы полей ButtonField из списка в верхнем левом углу и нажмите кнопку "Добавить".


![Добавление двух полей Buttonfield к GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Рис. 18**: добавление двух полей Buttonfield к GridView


Переместите двух полей Buttonfield таким образом, чтобы они отображаются как первые два поля GridView. Затем задайте `Text` свойства этих двух полей Buttonfield к цена + 10% и цена -10% и `CommandName` свойства IncreasePrice и DecreasePrice, соответственно. По умолчанию ButtonField отображает свой столбец кнопок как элементов управления LinkButton. Это можно изменить, однако через ButtonField s [ `ButtonType` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Позвольте s у этих двух полей Buttonfield к просмотру как обычные Нажимаемые кнопки; Таким образом, задать `ButtonType` свойства `Button`. Рис. 19 показано поля диалоговое окно после внесения этих изменений; Далее приведен декларативная разметка GridView s.


![Настройка полей Buttonfield Text, CommandName и ButtonType свойства](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Рис. 19**: Настройка полей Buttonfield `Text`, `CommandName`, и `ButtonType` свойства


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

С помощью этих полей Buttonfield создан, последним шагом является создание обработчика событий для GridView s `RowCommand` событий. Этот обработчик событий, если сработала из-за либо цена + 10% или цена -10% кнопки щелкают, необходимо определить `ProductID` для строки, кнопка которой была нажата, а затем вызовите `ProductsBLL` класс s `UpdateProduct` метод, передавая соответствующий `UnitPrice` процентах вместе с идентификатором `ProductID`. Следующий код выполняет следующие задачи:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Чтобы определить `ProductID` для строки, цена + 10%» или «цена -10% кнопка была нажата, нам нужно обратитесь к GridView s `DataKeys` коллекции. Эта коллекция содержит значения полей, указанных в `DataKeyNames` свойство для каждой строки GridView. С момента GridView s `DataKeyNames` свойство имеет значение ProductID по Visual Studio при привязке элемента управления ObjectDataSource к GridView, `DataKeys(rowIndex).Value` предоставляет `ProductID` для указанного *rowIndex*.

ButtonField автоматически передает в *rowIndex* строки, кнопка которой была нажата, через `e.CommandArgument` параметра. Таким образом чтобы определить `ProductID` для строки, цена + 10%» или «цена -10% кнопка была нажата, мы используем: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Как с кнопкой "снять с продажи все продукты", при отключении состояния представления GridView s GridView лежащему в базовом хранилище данных при каждой обратной передаче и, следовательно будет немедленно обновлен, чтобы отразить изменение в цене, вызванное нажатием одной из кнопок. Если, однако вы не отключили состояние представления в GridView, необходимо будет вручную повторную привязку данных к GridView после внесения этого изменения. Чтобы выполнить это, просто отправьте вызов в GridView s `DataBind()` сразу же после вызова `UpdateProduct` метод.

Рис. 20 показана страница при просмотре продуктов, предоставляемых Grandma Kelly's Homestead. Рис. 21 показаны результаты после цена + 10% был выполнен щелчок кнопкой дважды, чтобы распределить Boysenberry сути и кнопки цена -10% один раз для Northwoods Cranberry Sauce.


[![GridView включает цена + 10% и цена -10% кнопки](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Рис. 20**: Цена включает в себя GridView + 10% и цена -10% кнопки ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Цены для продукта, первый и третий были обновлены с помощью цена + 10% и цена -10% кнопки](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Рис. 21**: цены на первый и третий продукта были обновлены с помощью цена + 10% и цена -10% кнопки ([Просмотр полноразмерного изображения](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> GridView (и DetailsView) также может иметь кнопок, элементов управления LinkButton или ImageButtons, добавить свои поля TemplateField. Как с помощью типа BoundField, при нажатии этих кнопок относящееся обратную передачу, вызов GridView s `RowCommand` событий. При добавлении кнопки в поле TemplateField, тем не менее, кнопка s `CommandArgument` не устанавливается автоматически на индекс строки, как при использовании полей Buttonfield. Если вам нужно определить индекс строки кнопки, которая была нажата внутри `RowCommand` обработчик событий, вам потребуется вручную настроить кнопку s `CommandArgument` свойство его декларативный синтаксис в TemplateField, с помощью следующего кода:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Сводка

GridView, DetailsView и FormView могут входить кнопки кнопок, элементов управления LinkButton или ImageButtons. При нажатии эти кнопки вызывает обратную передачу и вызывать `ItemCommand` событий в элементах управления FormView и DetailsView и `RowCommand` событий в GridView. Эти данные веб-элементы управления имеют встроенные функции для обработки типовых операций, относящихся к командам, таких как удаление или изменение записей. Тем не менее, мы также можно использовать кнопки, при щелчке, отвечать с выполнением собственного специального кода.

Для этого необходимо создать обработчик событий для `ItemCommand` или `RowCommand` событий. В этом обработчике событий мы сначала проверять входящее значение `CommandName` значение, чтобы определить, какая кнопка была нажата и предпринять соответствующие пользовательские действия. В этом учебнике мы рассмотрели использование кнопок и полей Buttonfield снять с продажи все продукты определенного поставщика, или для увеличения или уменьшения цены определенного продукта на 10%.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-and-responding-to-buttons-to-a-gridview-cs.md)
