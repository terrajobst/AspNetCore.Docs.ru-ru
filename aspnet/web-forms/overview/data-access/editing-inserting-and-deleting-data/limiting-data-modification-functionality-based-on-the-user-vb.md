---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Ограничение функций изменения данных на основе пользователя (Visual Basic) | Документы Microsoft
author: rick-anderson
description: В веб-приложении, пользователи могут изменять данные разных учетных записей пользователя могут иметь разные привилегии для изменения данных. В этом учебнике мы рассмотрим как t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: eccb8e114e0ecf772680a2e5b36a761736cf8025
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887877"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Ограничение функций изменения данных на основе пользователя (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) или [скачать PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> В веб-приложении, пользователи могут изменять данные разных учетных записей пользователя могут иметь разные привилегии для изменения данных. В этом учебнике мы рассмотрим, как динамически настраивать возможности модификации данных, в зависимости от посетителя.


## <a name="introduction"></a>Вступление

Число веб-приложений поддерживают учетные записи пользователей и обеспечивают различные параметры, отчеты и функциональных возможностей, основанных на пользователя. Например с нашим руководствам мы может потребоваться разрешить пользователям из компаний-поставщиков для входа на обновление сайта и общие сведения о продуктах - свое имя и количество на единицу, возможно, — а также сведения о поставщиках, название компании, адрес, сведения о контактном лице s и т. д. Кроме того необходимо включить некоторые учетные записи пользователей для людей с нашей компании, чтобы они могли войти в систему и обновить сведения о продукте, как единиц на складе, запас и т. д. Наш веб-приложения также могут разрешить анонимным пользователям посещать (лицам, не вошедшие в систему), но может ограничить их могут только просматривать данные. С такой пользователь учетной записи системы на месте мы хотим данных веб-элементов управления в страницах ASP.NET, обеспечивают вставку, изменение и удаление возможностей, подходит для текущего пользователя.

В этом учебнике мы рассмотрим, как динамически настраивать возможности модификации данных, в зависимости от посетителя. В частности мы создадим страницы, отображающей данные поставщиков в редактируемой DetailsView вместе с GridView, в которой перечислены продукты, предоставляемый данным поставщиком. Если пользователь, посетив страницу нашей компании, они могут: просмотреть все сведения о поставщике s; изменить свой адрес; и изменять информацию для любых продуктов, предоставляемый данным поставщиком. Если, однако пользователь из конкретной компании, они могут только просматривать и изменять свои собственные сведения об адресах и можно изменять только свои продукты, которые не были помечены как более не поддерживается.


[![Является сотрудником компании можно редактировать все сведения о поставщике s](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Рис. 1**: пользователя из s наш компании можно редактировать любой поставщик сведения ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Пользователь из конкретного поставщика может только просмотр и изменение сведений о](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**На рисунке 2**: пользователя из конкретного поставщика можно только на просмотр и редактирование сведений о ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


S позволяют начать работу!

> [!NOTE]
> ASP.NET 2.0 система членства s предоставляет стандартизированные, расширяемую платформу для создания, управления и проверка учетных записей пользователей. Так как изучение система членства выходит за рамки этих учебников, этого учебника вместо «имитаций» членства, позволяя анонимных пользователей для выбора, являются ли они от конкретного поставщика или из нашей компании. Дополнительные сведения о членстве, см. в разделе Мои [проверки ASP.NET 2.0 s членства, ролей и профиля](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) статью рядов.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Шаг 1: Чтобы пользователи могли указывать их права доступа

В реальных приложениях s учетной записи пользователя включает ли они проработали в компании или для конкретного поставщика, и эта информация должна быть программно доступными из наших страниц ASP.NET, когда пользователь вошел в систему на сайт. Эта информация может фиксируемое через систему ролей ASP.NET 2.0 s, как уровень пользователя сведения об учетной записи через систему профиля или через какие-либо пользовательские средства.

Поскольку целью данного учебника является демонстрация настройки возможности модификации данных, зависящих от пользователя, вошедшего в систему и не предназначен для демонстрации ASP.NET 2.0 s членство, роли и профиль систем, мы будем использовать механизм очень просто определить возможности для пользователей, посетив страницу - DropDownList, из которого пользователь может указывать, если они должны иметь возможность просматривать и изменять любые поставщики данных и, кроме того, что сведения конкретного поставщика s, их можно просматривать и изменять. Если пользователь указывает, что она можно просматривать и изменять все сведения о поставщике (по умолчанию), она постраничного просмотра всех поставщиков, отредактировать сведения адрес поставщика s и измените имя и количество на единицу для любых продуктов, предоставляемые выбранного поставщика. Если пользователь указывает, что она может просматривать только и изменение конкретного поставщика, тем не менее, а затем она только просмотр сведений и продуктов для этого один поставщик и можно только обновления имени и количество на единицу сведения для этих продуктов, являющихся *не* более не поддерживается.

Наш первый шаг в этом учебнике будет, для создания этого DropDownList и заполнить ее поставщиков в системе. Откройте `UserLevelAccess.aspx` страницы в `EditInsertDelete` папки, добавьте DropDownList которого `ID` свойству `Suppliers`и привязать этот элемент DropDownList новый ObjectDataSource с именем `AllSuppliersDataSource`.


[![Создать новый элемент управления ObjectDataSource AllSuppliersDataSource с именем](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Рис. 3**: создать новый именованный ObjectDataSource `AllSuppliersDataSource` ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Поскольку мы хотим этого DropDownList для включения всех поставщиков, настройте элемент управления ObjectDataSource для вызова `SuppliersBLL` класса s `GetSuppliers()` метод. Также убедитесь, что ObjectDataSource s `Update()` метода сопоставлен `SuppliersBLL` класса s `UpdateSupplierAddress` метода, как этот элемент управления ObjectDataSource будет также использоваться DetailsView, мы будем добавлять на шаге 2.

После завершения работы мастера ObjectDataSource выполните действия по настройке `Suppliers` DropDownList так, чтобы он показывал `CompanyName` поля данных и использует `SupplierID` поля данных в качестве значения для каждого `ListItem`.


[![Настройка DropDownList поставщики использование CompanyName и полей данных SupplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Рис. 4**: Настройка `Suppliers` DropDownList для использования `CompanyName` и `SupplierID` поля данных ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


На этом этапе DropDownList список названий компаний поставщиков в базе данных. Тем не менее необходимо также включить параметр «Показать или изменить все поставщики», чтобы DropDownList. Чтобы сделать это, задайте `Suppliers` DropDownList s `AppendDataBoundItems` свойства `true` и добавьте `ListItem` которого `Text` свойство — «Показать или изменить все поставщики», значение которого — `-1`. Их можно добавить непосредственно с помощью декларативной разметкой или с помощью конструктора в окне «Свойства» и щелкнув многоточие в DropDownList s `Items` свойство.

> [!NOTE]
> Обращаться к [ *иерархического фильтрации с DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) учебника более подробный рассказ о добавлении Выбор всех элементов в DropDownList привязки к данным.


После `AppendDataBoundItems` свойства и `ListItem` добавлен, DropDownList s должна выглядеть как:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Рис. 5 показан снимок экрана текущий ход работы, при просмотре через браузер.


[![Поставщики DropDownList содержит показа всех ListItem, а также один для каждого поставщика](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Рис. 5**: `Suppliers` DropDownList содержит Показать все `ListItem`, а также один для каждого поставщика ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Поскольку необходимо обновить пользовательский интерфейс, сразу после пользователь изменил свой выбор, установите `Suppliers` DropDownList s `AutoPostBack` свойства `true`. На шаге 2 мы создадим элемента управления DetailsView, отображающий сведения о supplier(s), основанного на выборе DropDownList. Затем, в шаге 3 мы создадим обработчик событий для этого s DropDownList `SelectedIndexChanged` событий, в котором мы добавим код, который связывает сведения о поставщиках соответствующие в DetailsView зависимости от выбранного поставщика.

## <a name="step-2-adding-a-detailsview-control"></a>Шаг 2: Добавление элемента управления DetailsView

Позволяет использовать для отображения сведений о поставщике DetailsView s. Для пользователей, которые можно просматривать и изменять все поставщики DetailsView будет поддерживать разбиение по страницам, позволяющее пользователю осуществлять пошаговое выполнение одной записи сведения поставщика одновременно. Если пользователь работает для конкретного поставщика, однако DetailsView будут показаны только этого конкретного поставщика сведения s и не будет содержать интерфейс разбиения по страницам. В любом случае необходимо разрешить пользователю изменять адрес s поставщика, города и страны поля DetailsView.

Добавить на страницу под DetailsView `Suppliers` DropDownList, задать его `ID` свойства `SupplierDetails`и привяжите его к `AllSuppliersDataSource` ObjectDataSource создан на предыдущем шаге. Затем установите флажки включить разбиение на страницы и разрешить изменение смарт-теге s DetailsView.

> [!NOTE]
> Если пункт не отображается параметр Разрешить изменение в DetailsView s смарт-тег его s, так как не соответствует ObjectDataSource s `Update()` метод `SuppliersBLL` класса s `UpdateSupplierAddress` метод. Теперь пора вернуться назад и внести эти конфигурационные изменения, после чего появится параметр Разрешить изменение в DetailsView s смарт-тег.


Поскольку `SuppliersBLL` класса s `UpdateSupplierAddress` метод принимает только четыре параметра - `supplierID`, `address`, `city`, и `country` -s DetailsView стояли изменения, чтобы `CompanyName` и `Phone` Стояли доступны только для чтения. Кроме того, удалить `SupplierID` BoundField полностью. Наконец `AllSuppliersDataSource` ObjectDataSource в настоящее время имеет его `OldValuesParameterFormatString` свойство `original_{0}`. Теперь пора значение этого свойства полном удалении из декларативного синтаксиса или присвоено значение по умолчанию `{0}`.

После настройки `SupplierDetails` DetailsView и `AllSuppliersDataSource` ObjectDataSource, у нас будет декларативный следующую разметку:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

На этом этапе DetailsView можно передавать сообщения через и информации об адресе выбранного поставщика s может быть обновлен, вне зависимости от выбора, сделанного в `Suppliers` DropDownList (см. рис. 6).


[![Все поставщики сведения можно просмотреть и обновить его адрес](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Рис. 6**: Any поставщики сведения можно просмотреть и обновить его адрес ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Шаг 3: Отображение только выбранного s сведения о поставщике

Нашу страницу в данный момент сведения для всех поставщиков, независимо от конкретного поставщика выбран из `Suppliers` DropDownList. Чтобы отобразить только сведения о поставщике для выбранного поставщика нам нужно добавить другой элемент управления ObjectDataSource на страницу, который извлекает сведения о конкретного поставщика.

Добавьте новый элемент управления ObjectDataSource страницу, присвоив ему имя `SingleSupplierDataSource`. Из его смарт-тег, щелкните ссылку настроить источник данных и его использование `SuppliersBLL` класса s `GetSupplierBySupplierID(supplierID)` метод. Как и в `AllSuppliersDataSource` ObjectDataSource, имеют `SingleSupplierDataSource` ObjectDataSource s `Update()` сопоставляется метод `SuppliersBLL` класса s `UpdateSupplierAddress` метод.


[![Настройка SingleSupplierDataSource ObjectDataSource использовать метод GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Рис. 7**: Настройка `SingleSupplierDataSource` ObjectDataSource для использования `GetSupplierBySupplierID(supplierID)` метод ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Далее мы повторно запрос на указание для источника параметра `GetSupplierBySupplierID(supplierID)` метод s `supplierID` входного параметра. Так как нам нужно показать сведения для поставщика, выбранного в элементе управления DropDownList используйте `Suppliers` DropDownList s `SelectedValue` свойство в качестве источника параметра.


[![Использовать поставщики DropDownList в качестве supplierID источник параметра](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Рис. 8**: использование `Suppliers` DropDownList как `supplierID` источник параметров ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Несмотря на это второй ObjectDataSource добавлены управления DetailsView в настоящее время настроен на использование всегда `AllSuppliersDataSource` ObjectDataSource. Необходимо добавить логику, чтобы настроить источник данных, используемый в DetailsView в зависимости от `Suppliers` элемент DropDownList для выбора. Чтобы сделать это, создайте `SelectedIndexChanged` обработчика событий для поставщиков DropDownList. Это самый простой способ могут создаваться, дважды щелкнув DropDownList в конструкторе. Этот обработчик событий должен определить, какой источник данных для использования и необходимо изменить привязку данных для DetailsView. Это достигается с помощью следующего кода:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Обработчик события начинается с определения, был ли выбран параметр «Показать или изменить все поставщики». Если он был, задает `SupplierDetails` DetailsView s `DataSourceID` для `AllSuppliersDataSource` и возвращает пользователя на первую запись в набор поставщиков, задав `PageIndex` значение 0. Если, однако пользователь выбрал конкретного поставщика в элементе управления DropDownList DetailsView s `DataSourceID` назначается `SingleSuppliersDataSource`. Независимо от того, какие данные используемый источник `SuppliersDetails` режим возвращается в режим только для чтения и привязываются DetailsView путем вызова `SuppliersDetails` управления s `DataBind()` метод.

Этот обработчик событий на месте элемента управления DetailsView теперь показывает выбранного поставщика, если был выбран параметр «Показать или изменить все поставщики», в этом случае всех поставщиков можно просмотреть с помощью интерфейса разбиения на страницы. Рис. 9 показана страница с параметром «Отображения и изменения всех поставщиков» установлен; Обратите внимание, что интерфейс разбиения по страницам присутствует, предоставляя пользователю возможность посетите и обновить любые поставщика. Рис. 10 показана страница с выбранного поставщика Ma Maison. Только Ma Maison s сведения в этом случае для просмотра и редактирования.


[![Все поставщики сведения можно просматривать и редактировать](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Рис. 9**: все поставщики сведения можно просмотреть и редактирования ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Можно просматривать и редактировать только сведения о поставщиках выбранного s](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Рис. 10**: только выбранного поставщика s сведения могут быть Viewed и редактирования ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> В этом учебнике DropDownList и DetailsView управлять s `EnableViewState` должно быть присвоено `true` (по умолчанию) из-за DropDownList s `SelectedIndex` и DetailsView s `DataSourceID` запомнить изменения свойств s, во время обратной передачи.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Шаг 4: Список поставщиков продуктов в изменяемого элемента управления GridView

С помощью полного DetailsView следующим шагом является включение изменяемого элемента управления GridView, перечислены продукты, предоставляемые выбранного поставщика. Это GridView следует разрешить редактирование только `ProductName` и `QuantityPerUnit` поля. Кроме того, если пользователь, посетив страницу конкретного поставщика, его следует разрешить только обновление этих продуктов, которые являются *не* более не поддерживается. Для этого необходимо сначала добавить перегрузку `ProductsBLL` класса s `UpdateProducts` метод, который принимает в только что `ProductID`, `ProductName`, и `QuantityPerUnit` поля в качестве входных данных. Мы хранить, шаг с заходом в результате процесса заранее многочисленные учебников позвольте s взгляните на здесь код, который следует добавить `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Эта перегрузка создан, мы готов для добавления элемента управления GridView и связанные методы. Добавить новый GridView на страницу, задайте его `ID` свойства `ProductsBySupplier`и настройте его для использования нового ObjectDataSource с именем `ProductsBySupplierDataSource`. Поскольку мы хотим этого GridView отображены продукты выбранной поставщиком, используйте `ProductsBLL` класса s `GetProductsBySupplierID(supplierID)` метод. Также сопоставить `Update()` метод к новому `UpdateProduct` перегрузку, мы только что создали.


[![Настройка с использованием перегрузки UpdateProduct только что созданный элемент управления ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Рис. 11**: Настройка ObjectDataSource для использования `UpdateProduct` перегружать только что создали ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Мы повторно предложено выбрать источник параметров для `GetProductsBySupplierID(supplierID)` метод s `supplierID` входного параметра. Поскольку мы хотим Показать продуктов для поставщика, выбранного в DetailsView, используйте `SuppliersDetails` управления DetailsView s `SelectedValue` свойство в качестве источника параметра.


[![Использовать в качестве источника параметра к свойству SelectedValue s SuppliersDetails DetailsView](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Рис. 12**: использование `SuppliersDetails` DetailsView s `SelectedValue` свойство в качестве источника параметра ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Возврат к GridView, удалите все поля GridView, за исключением `ProductName`, `QuantityPerUnit`, и `Discontinued`, Маркировка `Discontinued` CheckBoxField только для чтения. Кроме того установите флажок Разрешить изменение смарт-теге s GridView. После внесения этих изменений для GridView и ObjectDataSource должна выглядеть следующим образом:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Как и в наших предыдущих ObjectDataSources эту один s `OldValuesParameterFormatString` свойству `original_{0}`, которое может вызвать проблемы при попытке обновить название продукта s или количество на единицу. Полностью удалите это свойство из декларативного синтаксиса или задайте для него значение по умолчанию `{0}`.

По завершении этой конфигурации нашу страницу теперь перечислены продукты, предоставляемый данным поставщиком, выбранного в GridView (см. рис. 13). В настоящее время *любой* можно обновить имя продукта s или количество на единицу. Тем не менее необходимо обновить страницу логику, чтобы такие функциональные возможности запрещен для неподдерживаемые продукты для пользователей, связанных с конкретного поставщика. Мы исследуем в этом выпуске этот завершающий шаг на шаге 5.


[![Отображаются продукты, предоставляемый данным поставщиком выбран](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Рис. 13**: отображаются продукты, предоставляемый данным поставщиком выбран ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> С появлением этого изменяемого элемента управления GridView `Suppliers` DropDownList s `SelectedIndexChanged` обработчик событий должны быть обновлены до возврата к GridView в состояние только для чтения. В противном случае при выборе различных поставщиков в середине редактирования сведения о продукте с соответствующим индексом в GridView для нового поставщика также будет использоваться для редактирования. Чтобы предотвратить это, просто установите GridView s `EditIndex` свойства `-1` в `SelectedIndexChanged` обработчика событий.


Кроме того помните, что очень важно, что GridView, состояние представления s будет включен (по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, увеличивается риск того что параллельно работающие пользователи непреднамеренно удаление или изменение записей. В разделе [предупреждение: проблемы параллелизма с ASP.NET 2.0 GridViews/DetailsView/FormViews, поддерживают редактирование или удаление и с состояние просмотра отключено](http://scottonwriting.net/sowblog/posts/10054.aspx) для получения дополнительной информации.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Шаг 5: Запретить редактирование для неподдерживаемые продукты при отображения и изменения всех поставщиков — не выбран

Хотя `ProductsBySupplier` GridView будут обладать полной функциональностью, он в настоящее время предоставляет слишком много доступ к тем пользователям, которые берутся из конкретного поставщика. В нашем бизнес-правила таким пользователям необходимо обновила неподдерживаемые продукты. Чтобы этого добиться, мы можно скрыть (или отключить) «изменить» в этих строках GridView с неподдерживаемые продукты при посещений страницы пользователем от поставщика.

Создайте обработчик событий для GridView s `RowDataBound` событий. В этом обработчике событий необходимо определить, является ли пользователь связан с конкретного поставщика, который для этого учебника можно определить, проверив s DropDownList поставщики `SelectedValue` свойство — если его s, отличный от -1, то пользователь, связанный с конкретного поставщика. Для таких пользователей нам потребуется определить ли продукт не поддерживается. Мы можно взять ссылку фактического `ProductRow` экземпляра, привязанный к строке GridView через `e.Row.DataItem` свойства, как описано в [ *отображение сведения о в GridView s нижнего колонтитула* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Учебник. Если продукт не поддерживается, мы можно взять программный ссылку «Изменить» в GridView s CommandField методами, описанными в предыдущем учебнике [ *Добавление клиентского подтверждение при удалении* ](adding-client-side-confirmation-when-deleting-vb.md). После получения ссылки мы затем скрыть или отключить кнопку.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

С этим событием обработчика, при посещении этой страницы имени пользователя от определенного поставщика этих продуктов, которые не поддерживаются недоступны для редактирования, как кнопка правки скрыто для этих продуктов. Например s Креольская огненная смесь является неподдерживаемые продукты для нового Орлеана Cajun преимуществах поставщика. При посещении страницы для этого конкретного поставщика, «изменить» для данного продукта скрыта от видимости (см. рис. 14). Тем не менее при просмотре с помощью «Показать или изменить все поставщики», кнопка "Изменить" — доступны (см. рис. 15).


[![Для пользователей, зависящие от поставщика скрыт кнопку "Изменить" для s Креольская огненная смесь](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Рис. 14**: для пользователей определенного поставщика скрыт кнопку "Изменить" для s Креольская Gumbo Mix ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Для отображения и изменения всех пользователей, поставщики отображается кнопка изменить для s Креольская огненная смесь](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Рис. 15**: для отображения и изменения всех поставщиков пользователей, кнопку "Изменить" для отображения Gumbo Mix s Креольская ([Просмотр полноразмерное изображение](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Проверка наличия права доступа уровня бизнес-логики

В этом учебнике ASP.NET страницы обрабатывает всю логику в зависимости от того, какие сведения пользователь может видеть и какие продукты, которые он может обновить. В идеальном случае эту логику также будут на уровне бизнес-логики. Например `SuppliersBLL` класса s `GetSuppliers()` (который возвращает всех поставщиков) могут включать проверку, чтобы убедитесь, что текущему пользователю *не* связанные с какой-либо поставщик. Аналогичным образом `UpdateSupplierAddress` метод может включать в себя проверку, чтобы убедиться, что текущему пользователю, либо при работе нашей компании (и таким образом, можно обновить все сведения об адресах поставщики) или связан с поставщиком, данные которого обновляется.

Я не содержит такие проверки слоев уровень бизнес-ЛОГИКИ, так как в нашем руководстве права пользователя s определяются DropDownList на странице, что нет доступа к классам уровень бизнес-ЛОГИКИ. При использовании системы членства или один из out-встроенные схемы проверки подлинности, предоставляемый ASP.NET (например, проверка подлинности Windows), выполнившего s пользователя данные и сведения о роли может осуществляться из МЕТОДА, делая такой доступ права проверяет возможные в презентации и слои уровень бизнес-ЛОГИКИ.

## <a name="summary"></a>Сводка

Настройка интерфейса изменения данных, основанный на пользовательском должны большинства узлов, предоставляющих учетных записей пользователей. Пользователи с правами администратора можно удалить или изменить любые записи, в то время как обычных пользователей могут быть ограничены только обновление или удаление записей, которые они создали сами. Любой сценарий, возможно, данные веб-элементы управления ObjectDataSource, и классы уровня бизнес-логики можно расширить для добавления или запрещать определенные функции, на основе пользователя, вошедшего в систему. В этом учебнике мы узнали, как ограничить количество данных для просмотра и редактирования, в зависимости от того, пользователь связан с конкретного поставщика или если они работали для нашей компании.

Этот учебник завершает изучение вставки, обновления и удаления данных с помощью элементов управления GridView, DetailsView и FormView. Начиная с следующем уроке мы будем можем обратиться к Добавление разбиение по страницам и сортировка поддержки.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-client-side-confirmation-when-deleting-vb.md)
