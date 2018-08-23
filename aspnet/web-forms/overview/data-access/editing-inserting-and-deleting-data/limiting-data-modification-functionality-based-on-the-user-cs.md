---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Ограничение функций изменения данных на основе пользователя (C#) | Документация Майкрософт
author: rick-anderson
description: В веб-приложения, который позволяет пользователю изменять данные различные учетные записи пользователей могут иметь различные полномочия на изменение данных. В этом руководстве будет рассмотрен способ t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f54f8ef593363f9428b663051cc71b8ef4a2e67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829403"
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Ограничение функций изменения данных на основе пользователя (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) или [скачать PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> В веб-приложения, который позволяет пользователю изменять данные различные учетные записи пользователей могут иметь различные полномочия на изменение данных. В этом руководстве мы рассмотрим, как динамически корректировать возможности модификации данных, в зависимости от посетителя.


## <a name="introduction"></a>Вступление

Ряд веб-приложений поддерживает учетные записи пользователей и предоставления различных параметров, отчетов и функций в зависимости от текущего пользователя. Например ознакомившись с нашими руководствами мы можете дать пользователям из компаний-поставщиков для входа на обновление сайта и общие сведения о своих продуктах — их название и количество в единицах измерения, возможно, — а также сведения о поставщике, такие как имя своей компании, адрес, сведения о контактном лице s и т. д. Кроме того мы может потребоваться включить некоторые учетные записи пользователей для людей из нашей компании, чтобы они могли войти в систему и обновлять информацию о продуктах, как числа единиц на складе, минимального уровня запаса и т. д. Наше веб-приложение также позволит анонимным пользователям посетить (лицам, не подключавшиеся к системе), но при этом ограничивать их простым просмотром данных. С такой пользователь учетной записи системы на месте мы хотим данных веб-элементов управления на страницах ASP.NET, чтобы предложить Вставка, редактирование и удаление возможностей, подходит для текущего пользователя.

В этом руководстве мы рассмотрим, как динамически корректировать возможности модификации данных, в зависимости от посетителя. В частности мы создадим страницу, отображающую сведения suppliers в редактируемой DetailsView и GridView, которая перечисляет продукты, предоставляемые поставщиком. Если посещающий страницу пользователь из нашей компании, они могут: все данные, поставщик s; изменять их адреса; и изменять информацию для любой продукт, предоставляемый данным поставщиком. Если, однако пользователь из конкретной компании, они могут только просматривать и изменять свои собственные сведения об адресах и можно изменить только продукты, которые не были помечены как снятые с продажи.


[![Пользователь из нашей компании может изменить любые сведения о поставщике s](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Рис. 1**: пользователя из s наш компании можно редактировать любой поставщик сведения ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Пользователь с определенным поставщиком могут только просматривать и изменить свои сведения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Рис. 2**: пользователя от определенного поставщика можно только на просмотр и изменение информации о ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Позвольте s приступить к работе!

> [!NOTE]
> ASP.NET 2.0 система членства s предоставляет стандартизированную, расширяемую платформу для создания, управления и проверки учетных записей пользователей. Поскольку изучение системы членства выходит за рамки этих учебников, этот учебник «имитируется» членства, позволяя анонимным посетителям выбирать, являются ли они на определенного поставщика или из нашей компании. Дополнительные сведения о членстве, см. Мой [s ASP.NET 2.0, проверки членства, ролей и профиля](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) серия статей.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Шаг 1., Что позволяет пользователю указать их права доступа

В реальных приложениях s учетная запись пользователя будет включать ли они работали на нашу компанию или на определенного поставщика, и эта информация будет возможен программный доступ со страниц ASP.NET, когда пользователь вошел на сайт. Эту информацию можно зафиксировать через систему ролей ASP.NET 2.0 s, как данные учетной записи пользователя через систему профиля или некоторыми пользовательскими средствами.

Поскольку задачей данного учебного курса является демонстрация корректировки возможностей изменения данных, на основе пользователя, вошедшего в систему и не предназначен для членства s showcase ASP.NET 2.0, ролей и профиля систем, мы будем использовать механизм очень просто, чтобы определить, возможности для пользователя, посещающего страницу - элементе управления DropDownList, из которого пользователь может указать, если они должны иметь возможность просматривать и править информацию поставщиков или, кроме того, что сведения конкретного поставщика s, они могут просматривать и редактировать. Если пользователь указывает, что она может просматривать и править всю информацию поставщиков (по умолчанию), она можно перебирать всех поставщиков, изменить любые сведения о поставщике s адрес и измените имя и количество в единицах измерения любого продукта, предоставляемые выбранным поставщиком. Если пользователь указывает, что она может только просматривать и редактирования конкретного поставщика, тем не менее, а затем она только просматривать сведения и продукты для этого поставщика и можно только обновить имя и количество в информацию о единицах для тех продуктов, которые *не* более не поддерживается.

Нашим первым этапом в этом руководстве, то для создания этого элемента управления DropDownList и заполняет ее поставщиков в системе. Откройте `UserLevelAccess.aspx` странице в `EditInsertDelete` папки, добавление элемента управления DropDownList, `ID` свойству `Suppliers`и привязать к элементу управления ObjectDataSource с именем этого элемента управления DropDownList `AllSuppliersDataSource`.


[![Создайте новый ObjectDataSource, именуемый AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Рис. 3**: создайте новый ObjectDataSource с именем `AllSuppliersDataSource` ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Поскольку мы хотим этого элемента управления DropDownList содержались все поставщики, настройте ObjectDataSource на вызов `SuppliersBLL` класс s `GetSuppliers()` метод. Также убедитесь, что элемент управления ObjectDataSource s `Update()` относящимся `SuppliersBLL` класс s `UpdateSupplierAddress` метод, как данный элемент управления ObjectDataSource будет также использоваться DetailsView, мы добавим на шаге 2.

После завершения работы мастера ObjectDataSource, следуйте инструкциям по настройке `Suppliers` DropDownList таким образом, чтобы он показывал `CompanyName` и использовал поле данных `SupplierID` поля данных в качестве значения для каждого `ListItem`.


[![Настройте список DropDownList Suppliers использовать поля CompanyName и SupplierID данных](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Рис. 4**: Настройка `Suppliers` DropDownList для использования `CompanyName` и `SupplierID` полей данных ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


На этом этапе DropDownList список компании имен поставщиков в базе данных. Тем не менее необходимо также включить параметр «Show/Edit ALL Suppliers», чтобы DropDownList. Для этого необходимо задать `Suppliers` DropDownList s `AppendDataBoundItems` свойства `true` и добавьте `ListItem` которого `Text` свойство является «Show/Edit ALL Suppliers» и значение которого равно `-1`. Это можно сделать непосредственно через декларативную разметку или с помощью конструктора, перейдите в окно свойств и нажмите кнопку с многоточием в DropDownList s `Items` свойство.

> [!NOTE]
> Вернуться к [ *"основной/подробности" Фильтрация с помощью элемента управления DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) учебника более подробный рассказ о добавлении элемента выбрать все в привязкой к данным элемента управления DropDownList.


После `AppendDataBoundItems` свойства и `ListItem` добавлен, декларативная разметка элемента управления DropDownList s должен выглядеть так:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Рис. 5 показан снимок экрана для работы, при просмотре через браузер.


[![Список DropDownList Suppliers содержит показа всех ListItem, а также для каждого поставщика](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Рис. 5**: `Suppliers` DropDownList содержит Показать все `ListItem`, а также один для каждого поставщика ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Поскольку нам требуется обновить пользовательский интерфейс, сразу после пользователь изменил свой выбор, задайте `Suppliers` DropDownList s `AutoPostBack` свойства `true`. На шаге 2 мы создадим элемент управления DetailsView, который будет показывать информацию о поставщике(ах) на основе выбранного элемента управления DropDownList. Затем, на шаге 3, мы создадим обработчик событий для этого элемента управления DropDownList s `SelectedIndexChanged` событий, в котором мы добавим код, который привязывается к элементу управления DetailsView сведений о поставщике соответствующие зависимости от выбранного поставщика.

## <a name="step-2-adding-a-detailsview-control"></a>Шаг 2: Добавление элемента управления DetailsView

Разрешить использовать элементе управления DetailsView для отображения сведений о поставщике s. Для пользователей, которые можно просматривать и изменять все поставщики DetailsView будет поддерживать разбиение на страницы, позволяя пользователям перемещаться по одной записи сведения поставщика за раз. Если пользователь работает на определенного поставщика, однако DetailsView показывают только этом определенном поставщике сведения s и не будет содержать интерфейс разбиения по страницам. В любом случае необходимо разрешить пользователю изменять адрес s поставщика, Город и Страна поля DetailsView.

Добавление элемента DetailsView на страницу под `Suppliers` DropDownList, задайте его `ID` свойства `SupplierDetails`и привяжите его к `AllSuppliersDataSource` ObjectDataSource создан на предыдущем шаге. Затем установите флажки включить разбиение по страницам и разрешить редактирование в смарт-теге DetailsView s.

> [!NOTE]
> Если параметр Разрешить изменение в смарт-s DetailsView см. в разделе Дон t пометить ее s, так как не удалось сопоставить ObjectDataSource s `Update()` метод `SuppliersBLL` класс s `UpdateSupplierAddress` метод. Отвлекитесь и вернуться назад и внести эти конфигурационные изменения, после которого параметр Разрешить изменение должен появиться в смарт-теге DetailsView s.


Так как `SuppliersBLL` класс s `UpdateSupplierAddress` метод принимает только четыре параметра - `supplierID`, `address`, `city`, и `country` -s DetailsView поля BoundField, кроме изменения, чтобы `CompanyName` и `Phone` Поля BoundField, кроме доступны только для чтения. Кроме того, удалите `SupplierID` BoundField полностью. Наконец `AllSuppliersDataSource` ObjectDataSource в настоящее время имеет его `OldValuesParameterFormatString` свойство значение `original_{0}`. Отвлекитесь и этот параметр свойства полном удалении из декларативного синтаксиса или установить его значение по умолчанию `{0}`.

После настройки `SupplierDetails` DetailsView и `AllSuppliersDataSource` ObjectDataSource, у нас будет следующей декларативной разметке:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

На этом этапе DetailsView можно просматривать постранично и информации об адресе выбранного поставщика s могут обновляться, вне зависимости от выбора, сделанного в `Suppliers` DropDownList (см. рис. 6).


[![Все поставщики сведения можно просмотреть и обновить его адрес](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Рис. 6**: Any поставщики сведения можно просмотреть и обновить свой адрес ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Шаг 3: Отображение только выбранного s информации о поставщике

Нашей странице отображаются сведения обо всех поставщиках вне зависимости от того определенного поставщика был выбран из `Suppliers` DropDownList. Чтобы отобразить только сведения для выбранного поставщика, нам нужно добавить другой элемент управления ObjectDataSource к нашей странице, который будет извлекать информацию о определенном поставщике.

Добавить новый элемент управления ObjectDataSource на страницу, назовите его `SingleSupplierDataSource`. Из его смарт-теге щелкните ссылку Настройка источника данных, который будет использовать `SuppliersBLL` класс s `GetSupplierBySupplierID(supplierID)` метод. Как и в `AllSuppliersDataSource` ObjectDataSource, имеют `SingleSupplierDataSource` ObjectDataSource s `Update()` сопоставить с относящимся к `SuppliersBLL` класс s `UpdateSupplierAddress` метод.


[![Настройте элемент SingleSupplierDataSource ObjectDataSource на использование метода GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Рис. 7**: Настройка `SingleSupplierDataSource` ObjectDataSource для использования `GetSupplierBySupplierID(supplierID)` метод ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


Далее мы повторно запрос на указание источника параметра для `GetSupplierBySupplierID(supplierID)` метод s `supplierID` входного параметра. Поскольку мы хотим Показать сведения о поставщике, выбранном в элементе управления DropDownList, используйте `Suppliers` DropDownList s `SelectedValue` свойство как источник параметра.


[![Используйте список Suppliers DropDownList как источник параметра supplierID](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Рис. 8**: использование `Suppliers` DropDownList как `supplierID` источник параметра ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


Даже при наличии этой второй элемент управления ObjectDataSource добавлен, элемент управления DetailsView настроен всегда использовать `AllSuppliersDataSource` ObjectDataSource. Нам нужно добавить логику, чтобы скорректировать источник данных, используемые в зависимости от элемента управления DetailsView `Suppliers` выбран элемент DropDownList. Для этого создайте `SelectedIndexChanged` обработчик событий для элемента управления DropDownList Suppliers. Это проще всего могут создаваться двойным щелчком элемента управления DropDownList в конструкторе. Этот обработчик событий должен определить, какой источник данных для использования и заново привязать данные к элементу управления DetailsView. Это выполняется следующим кодом:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Обработчик событий начинает с определения, был ли выбран параметр «Show/Edit ALL Suppliers». Если Да, он задает `SupplierDetails` DetailsView s `DataSourceID` для `AllSuppliersDataSource` и возвращает пользователя к первой записи в наборе поставщиков, задав `PageIndex` значение 0. Если, однако пользователь выбрал определенного поставщика в элементе управления DropDownList, DetailsView s `DataSourceID` назначается `SingleSuppliersDataSource`. Независимо от того, какие данные используемый источник `SuppliersDetails` переводится в режим только для чтения и привязываются к элементу управления DetailsView путем вызова `SuppliersDetails` управления s `DataBind()` метод.

С этого обработчика событий в месте элементе управления DetailsView теперь Показывать выбранного поставщика, если был выбран параметр «Show/Edit ALL Suppliers», в этом случае всех поставщиков можно просмотреть через интерфейс разбиения по страницам. Рис. 9 показана страница с параметром «Show/Edit ALL Suppliers» выбран; Обратите внимание, что присутствует интерфейс разбиения по страницам, позволяя пользователю посетить и обновить любого поставщика. Рис. 10 показана страница с где выбран поставщик Ma. Только сведения о Ma где s — возможности просмотра и правки в этом случае.


[![Всю информацию поставщиков могут просматривать и редактировать](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Рис. 9**: все поставщики можно просмотреть данные и редактирования ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Можно просматривать и редактировать только сведения о выбранном поставщике s](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Рис. 10**: только для выбранного поставщика s сведения могут быть Viewed и редактирования ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> В этом учебнике DropDownList и DetailsView управления s `EnableViewState` должно быть присвоено `true` (по умолчанию) так как DropDownList s `SelectedIndex` и DetailsView s `DataSourceID` свойство s изменения должны сохраняться во время обратной передачи.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Шаг 4: Перечисление продуктов поставщика в изменяемого элемента управления GridView

С помощью DetailsView полный следующим этапом является включение изменяемого элемента управления GridView, перечисляются продукты, предоставляемые выбранным поставщиком. Этот GridView должен допускать изменение только `ProductName` и `QuantityPerUnit` поля. Кроме того, если посещающий страницу пользователь на определенного поставщика, он должен возможность обновлять лишь тех продуктов, которые *не* более не поддерживается. Для этого необходимо сначала добавить перегрузку `ProductsBLL` класс s `UpdateProducts` метода, принимающего в только что `ProductID`, `ProductName`, и `QuantityPerUnit` как входные данные. Мы ve проходили этот процесс заранее в многочисленных руководствах по, так что давайте s взгляните на код, который следует добавить к `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Перегрузка, мы будет готов для добавления элемента управления GridView и его связанный элемент управления ObjectDataSource. Добавление нового элемента управления GridView на страницу, задайте его `ID` свойства `ProductsBySupplier`и настройте его для использования нового источника ObjectDataSource с именем `ProductsBySupplierDataSource`. Поскольку мы хотим этот GridView отображены продукты поставщиков, используйте `ProductsBLL` класс s `GetProductsBySupplierID(supplierID)` метод. Также сопоставить `Update()` метода к новому `UpdateProduct` мы только что созданной перегрузки.


[![Настройка ObjectDataSource на использование только что созданной перегрузки UpdateProduct](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Рис. 11**: Настройка ObjectDataSource для использования `UpdateProduct` перегрузки только что создали ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


Мы повторно запрос на Выбор источника параметра для `GetProductsBySupplierID(supplierID)` метод s `supplierID` входного параметра. Поскольку мы хотим Показать продукты поставщика, выбранного в DetailsView, используйте `SuppliersDetails` управления DetailsView s `SelectedValue` свойство как источник параметра.


[![Используйте свойство SelectedValue s элемента управления suppliersdetails типа DetailsView как источник параметра](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Рис. 12**: использование `SuppliersDetails` DetailsView s `SelectedValue` свойство как источник параметра ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Возвращаясь к GridView, удалите все поля GridView, за исключением `ProductName`, `QuantityPerUnit`, и `Discontinued`, Маркировка `Discontinued` CheckBoxField только для чтения. Кроме того установите флажок Enable Editing в смарт-теге GridView s. После внесения этих изменений декларативная разметка для GridView и элемент управления ObjectDataSource должен выглядеть следующим образом:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Как и в наших предыдущих элементов управления ObjectDataSource, это один s `OldValuesParameterFormatString` свойству `original_{0}`, что вызовет проблемы при попытке обновить имя продукта s или количество в единицах измерения. Полностью удалите это свойство из декларативного синтаксиса или установить его на значение по умолчанию, `{0}`.

По завершении этих настроек нашей странице теперь перечислены продукты, предоставляемые поставщиком, выбранным в GridView (см. рис. 13). В настоящее время *любой* можно обновить имя продукта s или количество в единицах измерения. Тем не менее нам нужно обновить логику страницы, чтобы закрыть для снятых с производства продуктов для пользователей, связанных с определенным поставщиком. Это будет рассмотрено этот окончательный элемент на шаге 5.


[![Отображаются продукты, предоставляемые поставщиком выбранные](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Рис. 13**: отображаются продукты, предоставляемый данным поставщиком выбран ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> С добавлением этого изменяемого элемента управления GridView `Suppliers` DropDownList s `SelectedIndexChanged` обработчика событий необходимо обновить, чтобы вернуть GridView в состояние только для чтения. В противном случае если в ходе правки сведения о продукте выбран другой поставщик, соответствующий индекс в GridView для нового поставщика также станет изменяемым. Чтобы избежать этого, просто задайте GridView s `EditIndex` свойства `-1` в `SelectedIndexChanged` обработчик событий.


Кроме того помните, что очень важно, что GridView состояние представления s быть включен (по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, возникает риск случайного удаления или изменения записей параллельно работающими пользователями. См. в разделе [предупреждение: проблемы параллелизма с помощью ASP.NET 2.0 элементов управления GridView/DetailsView/FormView среды, поддерживающих правку и/или удаление и которых состояние просмотра отключено](http://scottonwriting.net/sowblog/posts/10054.aspx) Дополнительные сведения.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Шаг 5: Запретить редактирование неподдерживаемые продукты при Show/Edit ALL Suppliers — не выбран

Хотя `ProductsBySupplier` GridView будет полностью работоспособно, оно дает слишком широкий доступ пользователям, работающим на определенного поставщика. В наши бизнес-правила такие пользователи не должны может обновить снятых с производства продуктов. Чтобы обеспечить это, можно скрыть (или отключить) "Изменить" в тех строках GridView с производства продукты, когда страница посещается пользователем, с использованием от поставщика.

Создайте обработчик событий для GridView s `RowDataBound` событий. В этом обработчике событий нужно определить, связан ли пользователь с определенным поставщиком, что для этого руководства, можно определить, проверив список DropDownList Suppliers s `SelectedValue` свойство — если он является s, отличный от -1, а затем пользователь связанные с определенным поставщиком. Затем, для таких пользователей необходимо определить ли продукт снят с производства. Можно взять ссылку на фактический `ProductRow` экземпляра, привязанный к строке GridView, с помощью `e.Row.DataItem` свойства, как описано в [ *отображение сведения о в нижнем колонтитуле GridView s* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) Учебник. Если продукт снят с производства, можно взять программную ссылку "Изменить" в GridView s CommandField методами, описанными в предыдущем учебном курсе, [ *Добавление клиентского подтверждения при удалении* ](adding-client-side-confirmation-when-deleting-cs.md). Получив ссылку мы затем можно скрыть или отключить кнопку.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

С этим событием обработчика, при посещении данной страницы пользователя от определенного поставщика, снятые с производства продукты недоступны для редактирования, как "Изменить" скрывается для этих продуктов. Например Chef Anton s Gumbo Mix является снятый с производства продукт New Orleans Cajun Delights поставщика. При посещении страницы этого конкретного поставщика кнопка, кнопка Edit для этого продукта скрыта (см. рис. 14). Тем не менее при посещении, с использованием «Show/Edit ALL Suppliers», "Изменить" — доступна (см. рис. 15).


[![Для пользователей конкретного поставщика кнопка изменить для Chef Anton s Gumbo Mix скрыта](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Рис. 14**: для пользователей конкретного поставщика кнопка "Изменить" для Chef Anton s Gumbo Mix скрыта ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Для всех пользователей поставщики Show/Edit отображается кнопка редактирования для Chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Рис. 15**: для Show/Edit все поставщики пользователи, кнопку "Изменить" для Chef Anton s Gumbo Mix отображается ([Просмотр полноразмерного изображения](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Проверка прав доступа на уровне бизнес-логики

В этом руководстве ASP.NET страницы обрабатывает всю логику в зависимости от того, какие сведения может видеть пользователь и какие продукты он может обновлять. В идеале эта логика будет также присутствовать на уровне бизнес-логики. Например `SuppliersBLL` класс s `GetSuppliers()` (возвращающего всех поставщиков) может входить проверка для обеспечения текущего пользователя *не* связанные с определенным поставщиком. Аналогичным образом `UpdateSupplierAddress` может входить проверка того, что текущему пользователю, либо работал нашу компанию (и таким образом, можно обновить все сведения об адресах поставщики) или связан с поставщиком, данные которого обновляется.

Я не включил такие проверки уровня БИЗНЕС-логики, так как в нашем учебном курсе права пользователя s определяются DropDownList на странице, которая не может получить доступ к классам BLL. При использовании системы членства или одной из схем проверки подлинности out-готовые возможности, предоставляемые ASP.NET (например, проверка подлинности Windows), в настоящее время вошедшего пользователя s информации и сведений о ролях может осуществляться из BLL, что делает такой доступ права проверяет возможные в презентации и слои BLL.

## <a name="summary"></a>Сводка

Большинство узлов, предоставляющих учетные записи пользователей должны Настройка интерфейса изменения данных, в зависимости от текущего пользователя. Пользователи с правами администратора можно попытаться удалить или изменить любую запись, в то время как пользователям без прав администратора могут быть ограничены обновлением и удалением записей, созданных ими самими. Любой сценарий, возможно, данные веб-элемент управления ObjectDataSource, и классы уровня бизнес-логики, которые могут быть расширены для добавления или запрещать определенные функции в зависимости от вошедшего в систему пользователя. В этом учебнике мы рассмотрели для ограничения возможности просмотра и правки данных в зависимости от ли пользователь был связан с определенным поставщиком или если они работали нашей компании.

Этим учебным курсом завершается изучение вставки, обновления и удаления данных с помощью элементов управления GridView, DetailsView и FormView. Начиная со следующего учебного, мы обратим наше внимание на разбиение по страницам и обеспечения поддержки сортировки.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-client-side-confirmation-when-deleting-cs.md)
> [Вперед](an-overview-of-inserting-updating-and-deleting-data-vb.md)
