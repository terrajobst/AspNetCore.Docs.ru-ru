---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: "Общие сведения о вставки, обновления и удаления данных (C#) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике будет показано, как сопоставить Insert() ObjectDataSource, Update(), и классы методы Delete() методам уровень бизнес-ЛОГИКИ, а также как допустимости конфигурации..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e483c37cc773a7255f18c26bc3609d68f71dff7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Общие сведения о вставки, обновления и удаления данных (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) или [скачать PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> В этом учебнике мы рассмотрим, как сопоставить Insert() ObjectDataSource, Update(), а классы методы Delete() методам уровень бизнес-ЛОГИКИ, а также настройке элементов управления GridView, DetailsView и FormView для обеспечения возможности модификации данных.


## <a name="introduction"></a>Вступление

За последние несколько учебников мы рассмотрели, как отображать данные на странице ASP.NET с помощью элементов управления GridView, DetailsView и FormView. Эти элементы управления просто работать с данными, предоставленный для них. Как правило эти элементы управления доступ к данным, с помощью элемент управления источником данных, такие как элемент управления ObjectDataSource. Мы узнали, как элемент управления ObjectDataSource выступает в качестве учетной записи-посредника между страницей ASP.NET и базовых данных. Когда GridView необходимую для отображения данных, она вызывает его ObjectDataSource `Select()` метод, который в свою очередь вызывает метод из наших бизнес логики уровень (уровень бизнес-ЛОГИКИ), которая вызывает метод в соответствующие данные доступа слоя (DAL) адаптера таблицы, который в свою очередь отправляет `SELECT` запрос к базе данных Northwind.

Помните, что если мы создали TableAdapters DAL в [наш первый Учебник](../introduction/creating-a-data-access-layer-cs.md), Visual Studio автоматически добавляет методы для вставки, обновления и удаления данных из базовой таблице базы данных. Кроме того, в [Создание слой бизнес-логики](../introduction/creating-a-business-logic-layer-cs.md) мы разработали методы в МЕТОДА, которая произвела вызов в эти методы DAL модификации данных.

В дополнение к его `Select()` ObjectDataSource метод, также имеет `Insert()`, `Update()`, и `Delete()` методы. Как `Select()` метод, эти три метода, которые могут быть сопоставлены с методами в нижележащий объект. Во время настройки для вставки, обновления или удаления данных в элементах управления GridView, DetailsView и FormView предоставляют пользовательский интерфейс для изменения основных данных. Этот пользовательский интерфейс вызывает `Insert()`, `Update()`, и `Delete()` методы ObjectDataSource, затем запуская базового объекта, связанного с методов (см. рис. 1).


[![ObjectDataSource Insert() Update() и методы Delete() служат в качестве учетной записи-посредника в МЕТОДА](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Рис. 1**: ObjectDataSource `Insert()`, `Update()`, и `Delete()` методы использоваться в качестве учетной записи-посредника в МЕТОДА ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


В этом учебнике будет показано, как сопоставить ObjectDataSource `Insert()`, `Update()`, и `Delete()` методы в методах классов в МЕТОДА, а также настройке элементов управления GridView, DetailsView и FormView для обеспечения изменения данных возможности.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Шаг 1: Создание Insert, Update и Delete учебники веб-страницы

Прежде чем начать изучение как вставки, обновления и удаления данных, сначала давайте немного времени для создания страниц ASP.NET в данном проекте веб-сайта, он понадобится для этого учебника и далее несколько из них. Начните с добавления в новую папку с именем `EditInsertDelete`. Добавьте следующие страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Добавление страниц ASP.NET для учебников изменения связанных данных](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**На рисунке 2**: Добавление страницы ASP.NET для учебников изменения связанных данных


Как и в других папках, `Default.aspx` в `EditInsertDelete` папки будут перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Таким образом, добавьте этот пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений в режиме конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Рис. 3**: добавление `SectionLevelTutorialListing.ascx` пользовательский элемент управления `Default.aspx` ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Наконец, добавления страниц, как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после Настройка форматирования `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элементы для редактирования, вставки и удаления учебники.


![Карта сайта теперь содержит записи для редактирования, вставки и удаления учебники](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Рис. 4**: карта сайта теперь содержит записи для редактирования, вставки и удаления учебники


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Шаг 2: Добавление и настройка элемента управления ObjectDataSource

С момента GridView, DetailsView и FormView каждая отличаться в их возможности модификации данных и макета давайте рассмотрим каждый из них по отдельности. Вместо каждого элемента управления, используя свой собственный элемент управления ObjectDataSource, тем не менее, давайте просто создать один элемент управления ObjectDataSource, можно предоставить все примеры три элемента управления.

Откройте `Basics.aspx` страницы, перетащите элемент управления ObjectDataSource из области элементов в конструктор и щелкните ссылку настроить источник данных из его смарт-тег. Поскольку `ProductsBLL` является единственным классом уровень бизнес-ЛОГИКИ, предоставляющий редактирования, вставки и удаления методов, настройте ObjectDataSource с помощью этого класса.


[![Настройка ObjectDataSource с помощью класса ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Рис. 5**: Настройка ObjectDataSource для использования `ProductsBLL` класса ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


На следующем экране можно указать метод `ProductsBLL` ObjectDataSource сопоставлен класс `Select()`, `Insert()`, `Update()`, и `Delete()` , выбрав соответствующую вкладку и Выбор способа из раскрывающегося списка. Рис. 6, которая теперь должна быть знакома, сопоставляет ObjectDataSource `Select()` метод `ProductsBLL` класса `GetProducts()` метод. `Insert()`, `Update()`, И `Delete()` методы могут быть настроены, выбрав соответствующую вкладку из списка в верхней.


[![ObjectDataSource открыть все продукты](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Рис. 6**: У ObjectDataSource возврата всех продуктов ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Вкладки фигуры 7, 8 и 9 Показать ObjectDataSource UPDATE, INSERT и DELETE. Настройка вкладок, чтобы `Insert()`, `Update()`, и `Delete()` методы вызывают `ProductsBLL` класса `UpdateProduct`, `AddProduct`, и `DeleteProduct` методов, соответственно.


[![Сопоставить метод Update() ObjectDataSource методом UpdateProduct класса ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Рис. 7**: сопоставление ObjectDataSource `Update()` метод `ProductBLL` класса `UpdateProduct` метод ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Сопоставить метод Insert() ObjectDataSource методом AddProduct класса ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Рис. 8**: сопоставление ObjectDataSource `Insert()` метод `ProductBLL` Add класса `Product` метод ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Сопоставить метод Delete() ObjectDataSource методом DeleteProduct класса ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Рис. 9**: сопоставление ObjectDataSource `Delete()` метод `ProductBLL` класса `DeleteProduct` метод ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Возможно, вы заметили раскрывающихся списках на вкладках UPDATE, INSERT и DELETE на уже эти методы выбран. Это происходит благодаря использование `DataObjectMethodAttribute` который оформляет методы `ProducstBLL`. Например метод DeleteProduct имеет следующую сигнатуру:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

`DataObjectMethodAttribute` Атрибут указывает назначение каждого метода, будь то для выбора, вставки, обновления, или удаление и того, является ли это значение по умолчанию. По умолчанию эти атрибуты при создании классов уровень бизнес-ЛОГИКИ вам будет необходимо вручную выбрать методы обновления, вставки и удаление вкладок.

Убедившись, что соответствующие `ProductsBLL` методы сопоставляются ObjectDataSource `Insert()`, `Update()`, и `Delete()` методов, нажмите кнопку Готово, чтобы завершить работу мастера.

## <a name="examining-the-objectdatasources-markup"></a>Проверки разметки для элемента управления ObjectDataSource

После настройки ObjectDataSource его мастером, перейдите к представлению источника для просмотра созданного декларативная разметка. `<asp:ObjectDataSource>` Тег задает базовый объект и методы для вызова неуправляемого кода. Кроме того, существуют `DeleteParameters`, `UpdateParameters`, и `InsertParameters` , которые отображаются входные параметры для `ProductsBLL` класса `AddProduct`, `UpdateProduct`, и `DeleteProduct` методов:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource включает параметр для каждого из входных параметров для своих связанных методов, как список `SelectParameter` s присутствует, когда элемент управления ObjectDataSource настраивается для вызова метода select, который требуется входной параметр (например `GetProductsByCategoryID(categoryID)`). Как будет видно, значения для этих `DeleteParameters`, `UpdateParameters`, и `InsertParameters` устанавливаются автоматически, GridView, DetailsView и FormView до вызова элемента управления ObjectDataSource `Insert()`, `Update()`, или `Delete()` метод. Эти значения можно задать программно, при необходимости, также как мы обсудим в будущем учебника.

Побочным эффектом того с помощью мастера настройки для ObjectDataSource в том, что Visual Studio [свойство OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) для `original_{0}`. Значение этого свойства используется для включения исходных значений данных редактируемого и полезен в двух сценариях:

- Если при изменении записи, пользователи могут изменить значение первичного ключа. В этом случае новым значением первичного ключа и исходное значение первичного ключа необходимо указать, чтобы найти и будет иметь значение по соответствующим образом обновлены записи исходное значение первичного ключа.
- При использовании оптимистичного параллелизма. Оптимистического параллелизма — это методика, чтобы убедиться, что две одновременно работающих пользователей не перезаписать изменения, выполненные другой, а — раздела для будущих учебника.

`OldValuesParameterFormatString` Свойство указывает имя базового объекта update и delete методы для исходных значений входных параметров. Мы обсудим это свойство и его назначение более подробно при изучении оптимистичного параллелизма. Я загружать его сейчас, тем не менее, поскольку методы нашей МЕТОДА не ожидают исходные значения, и поэтому очень важно, что мы удалите это свойство. Если оставить `OldValuesParameterFormatString` свойство задано только значение по умолчанию (`{0}`) приведет к ошибке при попытке вызова элемента управления ObjectDataSource данных веб-элемент управления `Update()` или `Delete()` методы так, как будет ObjectDataSource попытка передать в обоих `UpdateParameters` или `DeleteParameters` указан, а также исходное значение параметров.

Если это еще не совсем снимите флажок в этом juncture, не беспокойтесь, мы рассмотрим это свойство и его в будущем учебника. Сейчас просто обязательно это объявление свойства полностью удаляется из декларативного синтаксиса или задать значение по умолчанию ({0}).

> [!NOTE]
> Если просто очистить `OldValuesParameterFormatString` значение свойства в окне свойств в режиме конструктора, свойство по-прежнему будет существовать в декларативного синтаксиса, но задать пустую строку. К сожалению, будут по-прежнему в результате этой проблемы, описанные выше. Таким образом, либо удалите полностью из декларативного синтаксиса, или в окне свойств свойство значение по умолчанию `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Шаг 3: Добавление веб-управления и настроить его для изменения данных

После добавлен на страницу и настройки ObjectDataSource все готово для добавления веб-элементы управления на страницу для отображения данных и предоставляют средства для конечного пользователя изменить его. Мы рассмотрим GridView, DetailsView и FormView отдельно, как эти данные веб-элементов управления отличаются по их возможности модификации данных и конфигурации.

Как будет показано в оставшейся части этой статьи, добавление, изменение очень простым, вставка и удаление поддержки через GridView, DetailsView и FormView управляет является очень простым, как проверка несколько флажков. Существует много тонкостей и крайние случаи, в реальных условиях, сделать предоставления таких функций, которые сложнее, чем просто выберите команду и нажмите кнопку. Этот учебник, тем не менее, занимается только доказав возможности модификации данных упрощен. Будущих учебниках будут рассмотрены проблемы, без сомнения проявит себя в реальных условиях.

## <a name="deleting-data-from-the-gridview"></a>Удаление данных из GridView

Запуск, перетащив элемент управления GridView с панели элементов в конструктор. Затем привязать ObjectDataSource к GridView, выбрав необходимую из раскрывающегося списка в смарт-теге элемента GridView. На этом этапе будет декларативная разметка GridView:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Привязка GridView ObjectDataSource через его смарт-тег имеет два преимущества:

- Стояли и CheckBoxFields создаются автоматически для каждого поля, возвращаемые ObjectDataSource. Кроме того, BoundField и в CheckBoxField свойства задаются на основе базового поля метаданных. Например `ProductID`, `CategoryName`, и `SupplierName` поля помечены только для чтения в `ProductsDataTable` и поэтому не должен иметь возможность обновлять при редактировании. Для размещения этого, эти стояли [свойства ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) присваиваются `true`.
- [Свойство DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) назначается ключевого поля базового объекта. Это важно, когда то уникальный с помощью GridView для редактирования или удаления данных, поскольку это свойство указывает, что поле (или набор полей) определяет каждую запись. Дополнительные сведения о `DataKeyNames` свойства, обращаться к [главный/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) учебника.

Хотя GridView могут быть привязаны к ObjectDataSource через окно свойств или декларативного синтаксиса, таким образом необходимо вручную добавить соответствующую BoundField и `DataKeyNames` разметки.

Элемент управления GridView предоставляет встроенную поддержку для изменения уровня строк и удаления. Настройка элемента управления GridView для поддержки удаления добавляет столбец кнопок Delete. Когда пользователь нажимает кнопку удаления для конкретной строки, обратная передача и GridView выполняет следующие действия:

1. Элемент управления ObjectDataSource `DeleteParameters` присваиваются значения
2. Элемент управления ObjectDataSource `Delete()` вызывается метод, удаление указанной записи
3. GridView число повторных привязок сам в элемент управления ObjectDataSource путем вызова его `Select()` метод

Значения, присвоенные `DeleteParameters` являются значениями `DataKeyNames` полей, для которого была нажата для удаления строки. Поэтому крайне важно, элемент управления GridView `DataKeyNames` правильно задать свойство. Если он отсутствует, `DeleteParameters` будет назначен `null` значение в шаге 1, который в свою очередь не приводит к любой удаленных записей на шаге 2.

> [!NOTE]
> `DataKeys` Коллекция хранится в состоянии элемента управления GridView s, это значит, что `DataKeys` значения будут запомнены через обратную передачу, даже если состояние представления GridView s был отключен. Тем не менее очень важно, состояние представления остается включенной GridViews, который поддерживает изменение или удаление (по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, редактирование и удаление поведение подойдет для одного пользователя, но при наличии параллельных пользователей, удаление данных, существует вероятность случайного может этих одновременных пользователей удалить или изменить записывает, что они t планировалось. См. в мой записи блога [предупреждение: проблемы параллелизма с ASP.NET 2.0 GridViews/DetailsView/FormViews, поддерживают редактирование или удаление и с состояние просмотра отключено](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), Дополнительные сведения.


Это же предупреждение также применяется к DetailsViews и FormViews.

Чтобы добавить возможности удаления GridView, перейти к его смарт-тег и установите флажок Разрешить удаление.


![Проверка включения, удаление флажок](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Рис. 10**: Проверьте Enable, удаление флажок


Установив флажок Разрешить удаление из смарт-тег добавляет CommandField GridView. CommandField отображает столбец в GridView с кнопками для выполнения одно или несколько из следующих задач: Выбор записи, редактирование и удаление записи. Ранее мы видели CommandField в действии отбор записей в [главный/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) учебника.

CommandField содержит ряд `ShowXButton` свойств, которые указывают, какие ряды кнопок отображаются в CommandField. С помощью флажков Разрешить удаление CommandField которого `ShowDeleteButton` свойство `true` будет добавлен в коллекцию столбцов GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

На этом этапе Верите или нет, мы закончили с добавлением поддержки удаление GridView! Как показано на рис. 11, при просмотре страницы браузера столбец кнопок Delete присутствует.


[![CommandField добавляет столбец кнопок Delete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Рис. 11**: CommandField добавляет столбец для удаления кнопки ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Если построение этого учебника с нуля самостоятельно, при тестировании на этой странице нажмите кнопку «Удалить» вызовет исключение. Продолжите чтение, чтобы узнать, почему эти исключений и способы их устранения.

> [!NOTE]
> Если вы отрабатываете загрузки этого учебника, эти проблемы уже был учтен. Тем не менее я рекомендую прочтите сведения, перечисленные ниже, для выявления проблем, которые могут возникнуть и подходящие решения.


Если при попытке удалить продукт, возникает исключение, сообщение о которой похож на "*ObjectDataSource 'ObjectDataSource1» не удалось найти не групповой метод 'DeleteProduct', который имеет параметры: productID, исходное\_ ProductID*, «скорее всего вы забыли удалить `OldValuesParameterFormatString` свойство из коллекции. С `OldValuesParameterFormatString` свойство указано, ObjectDataSource пытается передать как `productID` и `original_ProductID` входных параметров для `DeleteProduct` метод. `DeleteProduct`, однако принимает только один входной параметр, поэтому исключение. Удаление `OldValuesParameterFormatString` свойства (или присвойте ей значение `{0}`) указывает, что элемент управления ObjectDataSource пытается передать исходный входной параметр.


[![Убедитесь, что свойство OldValuesParameterFormatString очищен](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Рис. 12**: Убедитесь, что `OldValuesParameterFormatString` свойство имеет был очищен Out ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Даже если удалить `OldValuesParameterFormatString` свойство, вы по-прежнему будет вызвано исключение при попытке удалить продукт с сообщением: «*конфликт инструкции DELETE с ССЫЛОЧНОЕ ограничение "FK\_порядок\_подробные сведения \_Продуктов*.» База данных "Борей" содержит ограничения внешнего ключа между `Order Details` и `Products` таблицы, т. е продукта не удаляются из системы, если существует одна или несколько записей для него в `Order Details` таблицы. Поскольку каждый продукт в базе данных Northwind имеет по крайней мере одну запись `Order Details`, не удается удалить какие-либо продукты, пока мы сначала удалить сведения о записи продукта связанного заказа.


[![Ограничение внешнего ключа запрещает удаление продуктов](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Рис. 13**: ограничение Foreign Key запрещает удаление продуктов ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


В этом руководстве, давайте просто удалите все записи из `Order Details` таблицы. В реальном приложении необходимо либо:

- У другой экран для управления сведениями о порядке
- Дополнять `DeleteProduct` метод логику, чтобы удалить сведения о заказе указанного продукта
- Изменить запрос SQL, используемые адаптера таблицы для включения удаление указанного продукта сведения о заказе

Давайте просто удалите все записи из `Order Details` таблицы, чтобы обойти ограничение внешнего ключа. Перейдите в обозреватель серверов в Visual Studio, щелкните правой кнопкой мыши `NORTHWND.MDF` узел и выберите новый запрос. В окне запроса выполните приведенную ниже инструкцию SQL:`DELETE FROM [Order Details]`


[![Удалите все записи из таблицы Order подробные сведения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Рис. 14**: удалить все записи из `Order Details` таблицы ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


После очистки `Order Details` таблицы, нажав кнопку удаления удалит продукта без ошибок. Если нажать кнопку удаления не удаляет продукта, проверьте, убедитесь, что в GridView `DataKeyNames` задано значение поля первичного ключа (`ProductID`).

> [!NOTE]
> При нажатии на кнопку удаления обратная передача и запись удаляется. Это может быть опасно, поскольку легко случайно нажмите кнопку Удалить неправильный строки. В будущем учебнике мы рассмотрим способы добавления клиентского подтверждения при удалении записи.


## <a name="editing-data-with-the-gridview"></a>Изменение данных с помощью GridView

Вместе с удалением, элемент управления GridView также предоставляет встроенную поддержку редактирования на уровне строк. Настройка элемента управления GridView, чтобы обеспечить редактирование добавляет столбец кнопок изменения. С точки зрения конечного пользователя, щелкнув строки редактирования кнопка причин, по которым строки станет доступно для редактирования превращается в ячейках в текстовые поля, содержащего существующие значения и замена кнопка правки с обновлением, а также кнопки "Отмена". После внесения нужного изменения, конечный пользователь может щелкните кнопкой "Обновить", чтобы сохранить изменения, или кнопку "Отмена", чтобы отменить их. В любом случае после нажатия кнопки обновления или "Отмена" GridView возвращает состояние до редактирования.

С нашей точки зрения разработчик страницы когда пользователь нажимает кнопку Изменить в определенной строке, обратная передача и GridView выполняет следующие действия:

1. GridView `EditItemIndex` присваивается индекс строки, для которого была нажата для редактирования
2. GridView число повторных привязок сам в элемент управления ObjectDataSource путем вызова его `Select()` метод
3. Индекс строки, который соответствует `EditItemIndex` подготавливается к просмотру в «режиме редактирования». В этом режиме «изменить» заменяется кнопками Обновить и Отмена и стояли которого `ReadOnly` свойства имеют значение False (по умолчанию) подготавливаются к просмотру как элементы управления TextBox Web которого `Text` свойства присваиваются значения полей данных.

На этом этапе разметку возвращается браузеру, что позволяет конечным пользователям вносить изменения в данные строк. При нажатии кнопки "Обновить", обратная передача и GridView выполняет следующие действия:

1. Элемент управления ObjectDataSource `UpdateParameters` значения присваиваются значения, введенные пользователем в интерфейс редактирования элемента GridView
2. Элемент управления ObjectDataSource `Update()` вызывается метод, обновление указанной записи
3. GridView число повторных привязок сам в элемент управления ObjectDataSource путем вызова его `Select()` метод

Значения первичного ключа, назначенный `UpdateParameters` в шаге 1 берутся из значений, указанных в `DataKeyNames` свойство, в то время как значения ключа неосновной берутся из текста в текстовом поле элементах для редактируемой строки. Как удалить, крайне важно, элемент управления GridView `DataKeyNames` правильно задать свойство. Если он отсутствует, `UpdateParameters` значение первичного ключа будет назначен `null` значение в шаге 1, который в свою очередь не влияет на все обновленные записи на шаге 2.

Функциональные возможности редактирования может быть активирован, просто установив флажок Разрешить изменение в смарт-теге элемента GridView.


![Проверка включения редактирования флажок](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Рис. 15**: Проверьте включение, изменение флажок


Проверка флажок Разрешить изменение добавит CommandField (при необходимости) и задайте его `ShowEditButton` свойства `true`. Как было сказано ранее, CommandField содержит ряд `ShowXButton` свойств, которые указывают, какие ряды кнопок отображаются в CommandField. Установив флажок Разрешить изменение добавляет `ShowEditButton` свойства существующего CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Это все, о добавлении низкий уровень поддержки для редактирования. Как показано Figure16 интерфейс редактирования вместо грубый каждого BoundField которого `ReadOnly` свойству `false` (по умолчанию) подготавливается к просмотру как элемент управления TextBox. Это включает поля, как `CategoryID` и `SupplierID`, которые являются ключами для других таблиц.


[![Нажав кнопку "Изменить" s Chai строки отображаются в режиме редактирования](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**На рисунке 16**: s Chai, нажав кнопку "Изменить" отображает строку в режиме правки ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Помимо попросить пользователей для непосредственного редактирования значений внешнего ключа, интерфейс интерфейс редактирования хватает одним из следующих способов:

- Если пользователь вводит `CategoryID` или `SupplierID` , которая не существует в базе данных, `UPDATE` нарушают ограничение внешнего ключа, вызывает исключение.
- Интерфейс редактирования не содержит каких-либо проверок. Если не указано обязательное значение (например, `ProductName`), или введите строковое значение, где ожидается числовое значение (например, введите «Слишком много!» в `UnitPrice` текстовое поле), будет вызвано исключение. Будущие учебника будут рассмотрены способы добавления элементов управления проверки пользовательский интерфейс редактирования.
- В настоящее время *все* поля продукта, которые не только для чтения, которые должны быть включены в GridView. Если удалить поле из GridView, скажем `UnitPrice`, если не установить обновление данных GridView `UnitPrice` `UpdateParameters` значение, которое будет изменять записи базы данных `UnitPrice` для `NULL` значение. Аналогично, если обязательное поле, такие как `ProductName`, удаляется из GridView, обновление завершится ошибкой, с тем же»*столбец «ProductName» не допускает значения NULL*"исключение, упомянутых выше.
- Интерфейс редактирования форматирование оставляет много необходимое. `UnitPrice` Показано с четырьмя знаками после запятой. В идеале `CategoryID` и `SupplierID` значения будет содержать элементами управления DropDownList, список категорий и поставщики в системе.

Ниже приведены все недостатки, которые придется смириться с сейчас, но будет устранена в будущих учебниках.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Вставка, изменение и удаление данных в DetailsView

Как видно в предыдущих учебниках управления DetailsView отображает по одной записи за раз и, подобно GridView, допускает редактирование и удаление текущей записи. Оба пользователя опыт работы с редактирование и удаление элементов из DetailsView и рабочий процесс на стороне ASP.NET идентична GridView. Когда DetailsView отличается от GridView — он также предоставляет встроенную поддержку вставки.

Чтобы продемонстрировать возможности модификации данных элемента управления GridView, начните с добавления DetailsView для `Basics.aspx` страницы выше существующего GridView и привязать его к существующей ObjectDataSource через DetailsView смарт-тега. Далее очистить DetailsView `Height` и `Width` свойства и установите флажок Включить разбиение на страницы из смарт-тега. Чтобы включить редактирование, вставка и удаление поддержки, просто установите флажки Разрешить изменение, разрешить вставку и разрешить удаление смарт-тега.


![Настройка DetailsView для поддержки редактирования, вставки и удаления](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Рисунок 17**: Настройка DetailsView для поддержки редактирования, вставки и удаления


Как и при GridView, Добавление изменение, вставка или удаление поддержки добавляет CommandField DetailsView, как показано в следующем декларативного синтаксиса:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Обратите внимание, что для DetailsView CommandField по умолчанию отображается в конце коллекции столбцов. Поскольку поля DetailsView отображаются как строки, CommandField появляется как строка с инструкцией Insert, изменение и удаление кнопок в нижней части DetailsView.


[![Настройка DetailsView для поддержки редактирования, вставки и удаления](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**На рисунке 18**: Настройка DetailsView для поддержки редактирования, вставки и удаления ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Щелкните «Удалить» запускает ту же последовательность событий с помощью GridView: a обратной передачи; следуют DetailsView, заполнение ее ObjectDataSource `DeleteParameters` на основе `DataKeyNames` значениям; и завершена при вызове его ObjectDataSource `Delete()` метод, который фактически удаляет продукта из базы данных. Редактирование в DetailsView также работает таким образом, идентична GridView.

Для вставки, конечный пользователь получает новый кнопка, при нажатии отображает DetailsView в «режиме вставки». С «режим вставки» «создать» заменяется Insert и "Отмена", а только те стояли которого `InsertVisible` свойству `true` (по умолчанию), отображаются. Поля данных определены как поля с автоматическим приращением, таких как `ProductID`, имеют свои [свойство InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) значение `false` при привязке к источнику данных через смарт-тег в DetailsView.

При привязке источника данных для DetailsView через смарт-тег, Visual Studio устанавливает `InsertVisible` свойства `false` только для полей с автоматическим приращением. Поля только для чтения, таких как `CategoryName` и `SupplierName`, будет отображаться в пользовательском интерфейсе «режим вставки», если их `InsertVisible` явно задано значение `false`. Теперь пора установить эти два поля `InsertVisible` свойства `false`, либо посредством декларативного синтаксиса DetailsView или изменить поля ссылку в смарт-тег. На рисунке 19 показано, как задать `InsertVisible` свойства `false` по ссылке на изменить поля.


[![Northwind Traders теперь предлагает Acme чая](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**На рисунке 19**: Northwind Traders теперь предлагает Acme чая ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


После установки параметра `InsertVisible` свойств, представление `Basics.aspx` в браузере и нажмите кнопку «Создать». Рис. 20 показано DetailsView при добавлении нового Напитки, чая Acme, нашей линейке продукции.


[![Northwind Traders теперь предлагает Acme чая](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Рис. 20**: Northwind Traders теперь предлагает Acme чая ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


После ввода сведений для Acme чая и нажав кнопку "Добавить", обратная передача и добавляется новая запись `Products` таблицы базы данных. Так как этот DetailsView перечислены продукты, в порядке, с которым они существуют в таблице базы данных, мы должен страницы до последнего продукта для просмотра нового продукта.


[![Подробные сведения об Acme чая](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Рисунок 21**: подробности чая Acme ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsView [свойство CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) указывает интерфейс, который будет отображаться, и может принимать одно из следующих значений: `Edit`, `Insert`, или `ReadOnly`. [DefaultMode свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) указывает режима DetailsView возврата после изменения или вставки был завершен и может использоваться для отображения DetailsView, который постоянно находится в режиме изменения или режим вставки.


Точку и щелкните Вставка и изменение возможности DetailsView приводит к снижению те же ограничения, что GridView: пользователь должен ввести существующих `CategoryID` и `SupplierID` значений с помощью текстового поля; отсутствует интерфейс логики проверки; все поля продукта, которые не допускают `NULL` значений нет значения по умолчанию или значение, указанное на уровне базы данных должны быть включены в Вставка интерфейса и т. д.

Методы будут рассмотрены для расширения и улучшение GridView интерфейс редактирования в будущем для элемента управления DetailsView интерфейсы правки и вставки также может применяться статей.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>С помощью FormView для более гибкий пользовательский интерфейс изменения данных

FormView обеспечивает встроенную поддержку для вставки, редактирования и удаления данных, но так как он использует шаблоны вместо поля отсутствует расположение для добавления стояли или CommandField, используемым элементами управления GridView и DetailsView для предоставления данных интерфейс для изменения. Вместо этого данным интерфейсом веб-элементы управления для сбора пользователя ввод при добавлении нового элемента или редактировании уже существующей, а также создать, изменить, удалить, вставки, обновления и кнопки отмены необходимо добавить вручную в соответствующих шаблонов. К счастью Visual Studio автоматически создаст необходимый интерфейс при привязке к источнику данных через раскрывающегося списка в смарт-теге FormView.

Чтобы продемонстрировать эти методы, начните с добавления FormView для `Basics.aspx` страницы и FormView смарт-теге, привязать его к ObjectDataSource уже создан. Это создаст `EditItemTemplate`, `InsertItemTemplate`, и `ItemTemplate` для FormView TextBox веб-элементами управления для сбора входных данных пользователя и кнопку веб-элементы управления для создания, изменения, удаления, вставки, обновления и кнопки отмены. Кроме того, FormView `DataKeyNames` задано значение поля первичного ключа (`ProductID`) объекта, возвращенные ObjectDataSource. Наконец установите флажок Включить постраничный просмотр в FormView смарт-тега.

В следующем примере показаны декларативная разметка для FormView `ItemTemplate` после привязки к элементу ObjectDataSource FormView. По умолчанию каждое поле продукта нелогических значение привязано к `Text` свойство метки веб-элемента управления во время каждого поля логическое значение (`Discontinued`) привязан к `Checked` свойство отключенный флажок веб-элемента управления. Чтобы активировать определенное поведение FormView при нажатии кнопки New, Edit и Delete, крайне важно, их `CommandName` присваивать значения `New`, `Edit`, и `Delete`соответственно.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

На рисунке 22 показана FormView `ItemTemplate` при просмотре через браузер. Каждое поле продукта указывается с помощью New, изменение и удаление кнопок в нижней.


[![ItemTemplate FormView по умолчанию перечислены все поля продукта, а также новые, изменение и удаление кнопок](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**На рисунке 22**: по умолчанию FormView `ItemTemplate` перечислены каждый продукт поля вдоль создать, изменить и удалить кнопок ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Как и с GridView и DetailsView, нажав кнопку «Удалить» или любой кнопки, LinkButton или ImageButton которого `CommandName` свойство имеет значение Delete вызывает обратную передачу, заполняет элемент управления ObjectDataSource `DeleteParameters` основании FormView `DataKeyNames`значение и вызывает элемент управления ObjectDataSource `Delete()` метод.

Когда нажата кнопка "Изменить" обратная передача и привязываются к `EditItemTemplate`, который отвечает за визуализации интерфейса редактирования. Этот интерфейс включает веб-элементы управления для редактирования данные вместе с кнопками Обновить и Отмена. Значение по умолчанию `EditItemTemplate` созданные Visual Studio содержит метку для всех полей с автоматическим приращением (`ProductID`), текстовое поле для каждого поля, отличного от логического значения и флажок для каждого поля логическое значение. Это поведение аналогично стояли создан в элементах управления GridView и DetailsView.

> [!NOTE]
> Небольшие проблемы со FormView автоматического создания `EditItemTemplate` является обработки TextBox Web элементы управления для полей, которые доступны только для чтения, таких как `CategoryName` и `SupplierName`. Мы рассмотрим с учетом этого чуть ниже.


Элементы управления TextBox в `EditItemTemplate` имеют свои `Text` свойство привязано к значению, их соответствующие поля данных при помощи *двухсторонней привязкой данных*. Двусторонняя привязка к данным, обозначенное с помощью `<%# Bind("dataField") %>`, выполняет привязку данных как во время привязки данных в шаблон, а также при заполнении параметров ObjectDataSource для вставки или изменения записей. То есть, при нажатии кнопку изменения из `ItemTemplate`, `Bind()` метод возвращает значение указанного поля. После пользователь делает их изменения, а последнее обновление значения обратной, соответствующие полям данных, заданные с помощью `Bind()` применяются для элемента управления ObjectDataSource `UpdateParameters`. Кроме того, односторонние привязки данных, обозначенное с помощью `<%# Eval("dataField") %>`, только извлекает значения полей данных при привязке данных к шаблону и выполняет *не* возврата значений, введенных пользователем в параметры источника данных при обратной передаче.

Следующие декларативная разметка показывает FormView `EditItemTemplate`. Обратите внимание, что `Bind()` метод используется в выражение привязки данных и элементы управления, обновление и Отмена веб-кнопка имеют свои `CommandName` заданы соответствующим образом.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Наш `EditItemTemplate`, на этой точки, вызовет исключение, при попытке использовать его. Проблема заключается в том `CategoryName` и `SupplierName` поля подготавливаются к просмотру как элементы управления TextBox Web в `EditItemTemplate`. Необходимо либо изменить эти текстовые поля, метки или полностью удалить. Давайте просто удалить их из `EditItemTemplate`.

На рисунке 23 показано FormView в браузере после нажатия на кнопку Правка для товара. Обратите внимание, что `SupplierName` и `CategoryName` поля, показанные на `ItemTemplate` больше не существует, как мы удалили их из `EditItemTemplate`. При нажатии кнопки "Обновить" FormView проходит через та же последовательность шагов, как элементы управления GridView и DetailsView.


[![По умолчанию EditItemTemplate показывает каждого продукта для редактирования поля как текстовое поле или флажок](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**На рисунке 23**: по умолчанию `EditItemTemplate` показывает каждый редактируемые поля Product как текстовое поле или флажок ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


При нажатии кнопки "Вставить" FormView `ItemTemplate` выполняется обратная. Тем не менее так как новая запись добавляется данные не привязан к FormView. `InsertItemTemplate` Интерфейс включает веб-элементы управления для добавления новой записи, а также кнопки вставки и отмены. Значение по умолчанию `InsertItemTemplate` созданные Visual Studio содержит текстовое поле для каждого поля, отличного от логического значения и флажок для каждого поля логическое значение, аналогично автоматически сформированного `EditItemTemplate`этого интерфейса. Элементы управления TextBox имеют свои `Text` свойство привязано к значению их соответствующего поля данных, с помощью двухсторонней привязкой данных.

Следующие декларативная разметка показывает FormView `InsertItemTemplate`. Обратите внимание, что `Bind()` метод используется в выражение привязки данных и элементы управления, вставки и отмены веб-кнопка имеют свои `CommandName` заданы соответствующим образом.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Нет тонкости с FormView автоматического создания `InsertItemTemplate`. В частности, элементы управления TextBox Web создаются даже для полей, которые доступны только для чтения, таких как `CategoryName` и `SupplierName`. Как и с `EditItemTemplate`, нам нужно удалить эти текстовые поля из `InsertItemTemplate`.

Рис. 24 показывает FormView в браузере при добавлении нового продукта Acme Coffee. Обратите внимание, что `SupplierName` и `CategoryName` поля, показанные на `ItemTemplate` больше не существует, как мы удалили их. При нажатии кнопки "Вставить" продолжается FormView через ту же последовательность шагов, как элемент управления DetailsView, добавив новую запись для `Products` таблицы. Рис. 25 показывает сведения о продукте Acme кофе в FormView после его вставки.


[![InsertItemTemplate определяет интерфейс Вставка FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Рис. 24**: `InsertItemTemplate` определяет интерфейс Вставка FormView ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Сведения для нового продукта Acme кофе, отображаются в FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Рис. 25**: сведений для нового продукта Acme кофе, отображаются в FormView ([Просмотр полноразмерное изображение](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


По отделить только для чтения, редактирование и вставка интерфейсов в три отдельные шаблоны FormView позволяет для более точного степень контроля над эти интерфейсы, чем DetailsView и GridView.

> [!NOTE]
> Как и DetailsView, FormView `CurrentMode` свойство указывает интерфейс, который будет отображаться и его `DefaultMode` указывает режим возвращает FormView, чтобы после изменения или вставки завершена.


## <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели основные принципы вставку, изменение и удаление данных с помощью GridView, DetailsView и FormView. Все три из этих элементов управления обеспечения некоторого уровня возможности модификации встроенных данных, которые можно использовать без написания ни единой строки кода на странице ASP.NET благодаря веб-элементы управления данными и ObjectDataSource. Тем не менее простой точку и нажмите кнопку методы отрисовки довольно frail и упрощенного данных изменения пользовательского интерфейса. Для выполнения проверки, внедрения программный значений, правильной обработки исключений, настройки пользовательского интерфейса и т. д., нам нужно будет полагаться на множество методов, которые обсуждаются через Далее несколько учебников.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Вперед](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
