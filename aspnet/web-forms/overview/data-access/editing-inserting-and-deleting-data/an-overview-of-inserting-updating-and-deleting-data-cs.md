---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Обзор вставки, обновления и удаления данных (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы будет показано, как сопоставить элементу ObjectDataSource Insert(), Update(), и классы Delete() методов к методам BLL, а также способы настройки...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e1b47782bd24824707266d1ed61e24789cc7a49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369842"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Обзор вставки, обновления и удаления данных (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) или [скачать PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> В этом руководстве мы будет показано, как сопоставить элементу ObjectDataSource Insert(), Update(), и классы Delete() методов к методам BLL, а также настройке элементов управления GridView, DetailsView и FormView для обеспечения возможностей изменения данных.


## <a name="introduction"></a>Вступление

Через нескольких последних учебных курсах мы изучали отображение данных на странице ASP.NET с помощью элементов управления GridView, DetailsView и FormView. Эти элементы управления просто работать с данными, которые им предоставлены. Часто эти элементы управления получить доступ к данным с помощью элементу управления источником данных, таких как элемент управления ObjectDataSource. Мы увидели, как ObjectDataSource действует как прокси между страницей ASP.NET и базовых данных. Когда GridView необходимые для отображения данных, он вызывает методы `Select()` метод, который в свою очередь вызывает метод из наших бизнес логики (BLL), которая вызывает метод в соответствующий доступ уровне данным (DAL) TableAdapter, который в свою очередь отправляет `SELECT` запроса к базе данных "Борей".

Помните, что при создании TableAdapter в DAL в [наш первый Учебник](../introduction/creating-a-data-access-layer-cs.md), Visual Studio автоматически добавляла методы для вставки, обновления и удаления данных из основного таблица базы данных. Кроме того, в [Создание слой бизнес-логики](../introduction/creating-a-business-logic-layer-cs.md) мы разработали методы в BLL, которая произвела вызов в эти методы DAL изменения данных.

В дополнение к его `Select()` метод, элемент управления ObjectDataSource также имеет `Insert()`, `Update()`, и `Delete()` методы. Как и `Select()` метод, эти три метода могут быть сопоставлены методам базового объекта. Во время настройки для вставки, обновления или удаления данных элементы управления GridView, DetailsView и FormView предоставляют пользовательский интерфейс для изменения базовых данных. Этот пользовательский интерфейс вызывает `Insert()`, `Update()`, и `Delete()` методов ObjectDataSource, которые затем вызвать базового объекта, связанного с методов (см. рис. 1).


[![Insert(), Update() и Delete() методов ObjectDataSource служат в качестве прокси в BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Рис. 1**: The ObjectDataSource `Insert()`, `Update()`, и `Delete()` методы служат в качестве учетной записи-посредника в BLL ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


В этом руководстве мы будет показано, как сопоставить ObjectDataSource `Insert()`, `Update()`, и `Delete()` методы в методах классов в BLL, а также настройке элементов управления GridView, DetailsView и FormView для обеспечения изменения данных возможности.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Шаг 1: Создание Insert, Update и Delete учебники веб-страниц

Прежде чем приступать, показывающих, как вставка, обновление и удаление данных, давайте немного, чтобы создавать страницы ASP.NET в нашем проекте веб-сайта, что нам понадобится для этого учебника и нескольких следующих. Начните с добавления новой папки с именем `EditInsertDelete`. Добавьте следующие страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Добавление страниц ASP.NET для данных учебных курсов по изменению](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Рис. 2**: Добавление страниц ASP.NET для данных учебных курсов по изменению


Как и в других папках, `Default.aspx` в `EditInsertDelete` папку перечислит учебные курсы в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Поэтому добавьте данный пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений в режиме конструктора.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Рис. 3**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Наконец, добавьте страницы как записи, чтобы `Web.sitemap` файл. В частности, добавьте следующую разметку после узла настраиваемого форматирования `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элементы для редактирования, вставки и удаления учебных курсов.


![Карта узла теперь включают записи для редактирования, вставки и удаления учебных курсов](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Рис. 4**: карта узла теперь включают записи для редактирования, вставки и удаления учебных курсов


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Шаг 2: Добавление и настройка элемента управления ObjectDataSource

С момента GridView, DetailsView и FormView, каждый по-разному их возможностей изменения данных и макета давайте рассмотрим каждый из них по отдельности. А не у каждого элемента управления, используя свой собственный элемент управления ObjectDataSource, тем не менее, просто создадим один элемент управления ObjectDataSource, можно предоставить все примеры три элемента управления.

Откройте `Basics.aspx` странице, перетащите элемент управления ObjectDataSource из панели элементов в конструктор и щелкните ссылку Настройка источника данных из его смарт-тега. Так как `ProductsBLL` — это единственный класс BLL, предоставляющий редактирования, вставки и удаления методов, настройте элемент ObjectDataSource для использования этого класса.


[![Настройка ObjectDataSource на использование класса ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Рис. 5**: Настройка ObjectDataSource для использования `ProductsBLL` класс ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


На следующем экране можно указать методы класса `ProductsBLL` класса сопоставляются с ObjectDataSource `Select()`, `Insert()`, `Update()`, и `Delete()` , выбрав соответствующую вкладку и выбрать метод стрелку раскрывающегося списка. Рис. 6, которая должна быть знакома к этому моменту, сопоставляет ObjectDataSource `Select()` метод `ProductsBLL` класса `GetProducts()` метод. `Insert()`, `Update()`, И `Delete()` методы можно настроить, выбрав соответствующую вкладку в списке в верхней.


[![Иметь элемент управления ObjectDataSource возвращает все продукты](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Рис. 6**: иметь элемент управления ObjectDataSource возвращает все продукты ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Рис. 7, 8 и 9 Показать ObjectDataSource UPDATE, INSERT и DELETE вкладки. Настройте эти вкладки, чтобы `Insert()`, `Update()`, и `Delete()` методы вызывают `ProductsBLL` класса `UpdateProduct`, `AddProduct`, и `DeleteProduct` методы, соответственно.


[![Сопоставление метода Update() элемента управления ObjectDataSource методу UpdateProduct класса ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Рис. 7**: сопоставление ObjectDataSource `Update()` метод `ProductBLL` класса `UpdateProduct` метод ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Сопоставление метода Insert() ObjectDataSource методу AddProduct класс ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Рис. 8**: сопоставление ObjectDataSource `Insert()` метод `ProductBLL` добавить класса `Product` метод ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Сопоставление метода Delete() ObjectDataSource методу DeleteProduct класса ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Рис. 9**: сопоставление ObjectDataSource `Delete()` метод `ProductBLL` класса `DeleteProduct` метод ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Вы могли заметить, что раскрывающихся списках на вкладках UPDATE, INSERT и DELETE эти методы уже выбран. Это обусловлено использованием из `DataObjectMethodAttribute` , установлен для методов класса `ProducstBLL`. Например методу DeleteProduct имеет следующую сигнатуру:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

`DataObjectMethodAttribute` Указывает назначение каждого метода, будь то для выбора, вставки, обновления, или удаление и ли его значение по умолчанию. Если при создании классов BLL эти атрибуты были пропущены, вы будет необходимо вручную выбрать методы обновления, вставки и удаления вкладок.

Убедившись, что соответствующий `ProductsBLL` сопоставлены методам ObjectDataSource `Insert()`, `Update()`, и `Delete()` методов, нажмите кнопку Готово, чтобы завершить работу мастера.

## <a name="examining-the-objectdatasources-markup"></a>Проверка разметки ObjectDataSource

После настройки ObjectDataSource с помощью мастера, перейдите в представление источника, чтобы проверить созданную декларативную разметку. `<asp:ObjectDataSource>` Тег задает базовый объект и вызываемые методы. Кроме того, существуют `DeleteParameters`, `UpdateParameters`, и `InsertParameters` , которые соответствуют входные параметры для `ProductsBLL` класса `AddProduct`, `UpdateProduct`, и `DeleteProduct` методов:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

Элемент управления ObjectDataSource включает параметр для каждого из входных параметров для соответствующих методов, так же, как список `SelectParameter` s присутствует, после настройки ObjectDataSource на вызов метода select, который требуется входной параметр (например `GetProductsByCategoryID(categoryID)`). Как скоро можно будет увидеть, их значения `DeleteParameters`, `UpdateParameters`, и `InsertParameters` задаются автоматически GridView, DetailsView и FormView до вызова ObjectDataSource `Insert()`, `Update()`, или `Delete()` метод. Эти значения можно задать программным образом, при необходимости, также будет рассмотрено в дальнейших учебных курсах.

Один побочный эффект использования мастера для настройки к ObjectDataSource в том, что Visual Studio [свойство OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) для `original_{0}`. Значение этого свойства используется для включения исходных значений данных, редактируемой и полезно в двух сценариях:

- Если при правке записи, пользователи могут изменять значение первичного ключа. В этом случае новым значением первичного ключа и исходным значением первичного ключа необходимо указать, чтобы найти и будет иметь значение по соответствующим образом обновлено записи с исходным значением первичного ключа.
- При использовании оптимистичного параллелизма. Оптимистичный параллелизм – методика убедитесь, что два одновременно работающих пользователей не перезаписать изменения друг друга и это тема будущего учебного курса.

`OldValuesParameterFormatString` Свойство указывает имя базового объекта update и delete методы для исходных значений входных параметров. Мы обсудим это свойство и его назначение, более подробно при изучении оптимистичного параллелизма. Я упомянул его сейчас, тем не менее, так как исходные значения методам BLL не ожидают, и поэтому очень важно, что мы удалим это свойство. Оставив `OldValuesParameterFormatString` свойство задано только значение по умолчанию (`{0}`) приведет к ошибке при попытке вызвать ObjectDataSource данных веб-элемента управления `Update()` или `Delete()` методы так, как ObjectDataSource будет попытка передать входные параметры `UpdateParameters` или `DeleteParameters` указан, а также исходные значения параметров.

Если это не очень на этом этапе, не беспокойтесь, мы рассмотрим это свойство и его программа в следующем учебном курсе. Пока просто не забудьте удалить это объявление свойства из декларативного синтаксиса или присвоено значение по умолчанию ({0}).

> [!NOTE]
> Если вы просто очистите `OldValuesParameterFormatString` значение свойства из окна свойств в режиме конструктора, свойство по-прежнему будет существовать в декларативном синтаксисе, но задать пустую строку. Это, увы, по-прежнему приведет та же проблема, описанных выше. Таким образом, либо удалите свойство полностью из декларативного синтаксиса или из окна свойств установите значение по умолчанию, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Шаг 3: Добавление веб-элемент данных и настроить его для изменения данных

После добавления на страницу и настройки ObjectDataSource мы готовы добавить веб-элементы управления на страницу, чтобы отобразить данные и предоставляют средства для конечного пользователя изменить его. Мы рассмотрим GridView, DetailsView и FormView отдельно, так как эти данные веб-элементов управления по-разному их возможностей изменения данных и конфигурации.

Как мы увидим в оставшейся части этой статьи, Добавление расширенных возможностей редактирования, вставки и удаления поддержка с помощью GridView, DetailsView и FormView является очень простой установкой нескольких флажков. Существует множество тонкостей и крайних случаев, в реальной жизни, делающих предоставление этих функций более сложным, чем просто выберите команду и нажмите кнопку. Этот учебник, тем не менее, рассматриваются только возможности модификации упрощенный данных. Последующих учебных курсах будут рассмотрены вопросы, которые непременно возникнут в реальных условиях.

## <a name="deleting-data-from-the-gridview"></a>Удаление данных из GridView

Для начала перетаскивания элемента управления GridView с панели инструментов в конструктор. Затем привяжите элемент управления ObjectDataSource к GridView, выбрав его из раскрывающегося списка в смарт-теге элемента GridView. На этом этапе декларативная разметка GridView будет:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Привязка элемента GridView к элементу ObjectDataSource через его смарт-тег имеет два преимущества:

- Поля BoundFields и CheckBoxFields создаются автоматически для каждого из полей, возвращаемые элементом ObjectDataSource. Кроме того BoundField и CheckBoxField элемента свойства задаются на основании метаданных базового поля. Например `ProductID`, `CategoryName`, и `SupplierName` поля помечены только для чтения в `ProductsDataTable` и поэтому не должно быть обновляемым при редактировании. Для размещения этого, эти поля BoundField, кроме [свойств только для чтения](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) присваивается `true`.
- [Свойство DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) назначается полей первичного ключа базового объекта. Это важно, когда то уникальный с помощью GridView для редактирования или удаления данных, поскольку это свойство указывает поле (или набор полей) определяет каждую запись. Дополнительные сведения о `DataKeyNames` свойство, обращаться к [Master/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) руководства.

Хотя GridView можно привязать к элементу ObjectDataSource через декларативный синтаксис или окно "Свойства", для этого необходимо вручную добавить соответствующую BoundField и `DataKeyNames` разметки.

Элемент управления GridView предоставляет встроенную поддержку редактирования на уровне строк и удаления. Настройка GridView для поддержки удаления добавляется столбец кнопок «Delete». Когда конечный пользователь нажимает кнопку «Удалить» для конкретной строки, обратная передача и GridView выполняет следующие действия:

1. ObjectDataSource `DeleteParameters` присваиваются значения
2. ObjectDataSource `Delete()` вызывается метод, удаление указанной записи
3. GridView повторную привязку к элементу ObjectDataSource, вызвав его `Select()` метод

Значения, присвоенные `DeleteParameters` являются значениями `DataKeyNames` поля, для которого была нажата для удаления строки. Поэтому крайне важно, элемента GridView `DataKeyNames` установлено правильно. Если он отсутствует, `DeleteParameters` будет назначен `null` значение на шаге 1, что в свою очередь не приведет любой удаленных записей на шаге 2.

> [!NOTE]
> `DataKeys` Коллекция хранится в состояние элемента управления GridView s, это означает, что `DataKeys` значения будут сохранены для обратной передачи, даже если состояние представления GridView s была отключена. Тем не менее очень важно состояния представления остается включенной для элементов управления GridView, который поддерживает изменение или удаление (поведение по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, поведение редактирования и удаления, хорошо подходит для одного пользователя, но если одновременно работающих пользователей, удаление данных, существует возможность, что этими пользователями может случайно Удаление или изменение записи, по которому t требуется. См. мою запись в блоге [предупреждение: проблемы параллелизма с помощью ASP.NET 2.0 элементов управления GridView/DetailsView/FormView среды, поддерживающих правку и/или удаление и которых состояние просмотра отключено](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), Дополнительные сведения.


Это предупреждение также относится к DetailsViews и FormView среды.

Для Удаление возможностей к GridView, просто перейдите к его смарт-тег и установите флажок "Разрешить удаление".


![Проверьте включение, удаление флажок](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Рис. 10**: Проверьте включение, удаление флажок


Установив в смарт-теге флажок Разрешить удаление добавлено поле добавляет CommandField к GridView. CommandField отображает столбец в GridView с кнопками для выполнения одной или нескольких из следующих задач: Выбор записи, редактирование и удаление записи. Ранее была рассмотрена CommandField в действии с помощью записей в [Master/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) руководства.

CommandField содержит ряд `ShowXButton` свойств, которые указывают, какие из набора кнопок изображаются CommandField. Установив флажок Разрешить удаление CommandField которого `ShowDeleteButton` свойство `true` был добавлен в коллекцию столбцов GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

На этом этапе Верите или нет, мы закончили Добавление поддержки удаления к элементу GridView! Как показано на рис. 11, при просмотре страницы через обозреватель столбец кнопок «Delete» присутствует.


[![CommandField добавляет столбец кнопок «Delete»](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Рис. 11**: CommandField добавляет столбец из удаление кнопки ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Если вы уже создаете этот учебник с нуля самостоятельно, при тестировании этой странице нажмите кнопку «Удалить» будет выдано исключение. Далее вы узнаете о том, почему возникают эти исключения и способы их устранения.

> [!NOTE]
> Если вы создаете пример вместе с помощью этого учебника, загрузки, эти проблемы уже учтены для. Тем не менее я рекомендую ознакомиться с указанных ниже с целью определения возможных проблем, возникающих, а также поиска подходящих обходных сведениям.


Если при попытке удаления продукта, возникнет исключение с сообщением, аналогичную "*ObjectDataSource 'ObjectDataSource1' не удалось найти неуниверсальный метод 'DeleteProduct' с параметрами: productID, исходное\_ ProductID*,» скорее всего вы забыли удалить `OldValuesParameterFormatString` свойства из элемента управления ObjectDataSource. С помощью `OldValuesParameterFormatString` свойство задано, ObjectDataSource пытается передать входные параметры `productID` и `original_ProductID` входные параметры `DeleteProduct` метод. `DeleteProduct`, однако принимает только один входной параметр, поэтому исключение. Удаление `OldValuesParameterFormatString` свойство (или присвойте ей значение `{0}`) указывает, что элемент управления ObjectDataSource пытается передать исходный входной параметр.


[![Убедитесь, что свойство OldValuesParameterFormatString был очищен](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Рис. 12**: Убедитесь, что `OldValuesParameterFormatString` свойство имеет был очищен Out ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Даже если удалить `OldValuesParameterFormatString` свойство, вы по-прежнему будет вызвано исключение при попытке удаления продукта с сообщением: "*конфликт инструкции DELETE с ССЫЛОЧНОЕ ограничение" FK\_порядок\_сведения \_Закупочного*.» Базы данных Northwind содержит ограничение внешнего ключа между `Order Details` и `Products` таблицы, это означает, что продукт нельзя удалить из системы, если один или несколько записей о нем в `Order Details` таблицы. Поскольку все продукты в базе данных Northwind имеют хотя бы одну запись `Order Details`, перед удалением любых продуктов, необходимо удалить записи сведений о связанных заказах продуктов.


[![Ограничение внешнего ключа препятствует удалению продуктов](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Рис. 13**: ограничение внешнего ключа запрещает удаление продуктов ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


В этом руководстве, просто удалим все записи из `Order Details` таблицы. В реальном приложении необходимо либо:

- У другого экрана, чтобы управлять сведений о порядке
- Дополнить `DeleteProduct` для включения логики, чтобы удалить сведения о заказе указанного продукта
- Изменение запроса SQL, используемого TableAdapter для включения удаления деталей заказа указанного продукта

Просто удалим все записи из `Order Details` Чтобы обойти ограничение внешнего ключа. Перейдите в проводник по серверам в Visual Studio, щелкните правой кнопкой мыши `NORTHWND.MDF` узел и выберите новый запрос. Затем в окне запроса выполните следующую инструкцию SQL: `DELETE FROM [Order Details]`


[![Удалите все записи из таблицы Order сведения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Рис. 14**: удалить все записи из `Order Details` таблицы ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


После очистки `Order Details` таблицы, нажав кнопку "Удалить" приведет к удалению продукта без ошибок. Если при нажатии кнопки Delete не приводит к удалению продукта, проверьте, чтобы убедиться, что GridView `DataKeyNames` свойству поле первичного ключа (`ProductID`).

> [!NOTE]
> При нажатии кнопки "Удалить" обратная передача и удаляется запись. Это может быть опасно, так как это легко случайно нажмите кнопку Удалить ошибиться. В следующем учебном курсе будет показано, как добавление клиентского подтверждения при удалении записи.


## <a name="editing-data-with-the-gridview"></a>Редактирование данных с помощью GridView

Помимо удаления, элемент управления GridView также предоставляет встроенную поддержку редактирования на уровне строк. Настройка GridView для поддержки редактирования добавляется столбец кнопок кнопки изменения. С точки зрения конечного пользователя, щелкнув строки редактирования кнопку причин, по которым строка становится изменяемой превратив ячейки в текстовые поля, содержащий существующие значения и заменить "Изменить", с обновлением и кнопки "Отмена". После внесения необходимых изменения, конечный пользователь может щелкнуть кнопкой "Обновить", чтобы сохранить изменения "или" Отмена ", чтобы отбросить их. В любом случае после нажатия кнопки обновления "или" Отмена "GridView возвращает состояние до редактирования.

С нашей точки зрения разработчика страницы когда конечный пользователь нажимает кнопку «Изменить» для конкретной строки, обратная передача и GridView выполняет следующие действия:

1. GridView `EditItemIndex` свойство назначено индекс строки, была нажата, кнопка "Изменить"
2. GridView повторную привязку к элементу ObjectDataSource, вызвав его `Select()` метод
3. Индекс строки, который соответствует `EditItemIndex` и отображается в «режиме правки». В этом режиме "Изменить" заменяется кнопки "обновления" и "Отмена" и поля BoundField, кроме которого `ReadOnly` свойства имеют значение False (по умолчанию) подготавливаются к просмотру как элементы управления TextBox Web, `Text` которых присвоены значения полей данных.

В этот момент разметка возвращается обозревателю, позволяя пользователю вносить изменения в данные строки. Когда пользователь щелкает кнопкой "Обновить", происходит обратная передача и GridView выполняет следующие действия:

1. ObjectDataSource `UpdateParameters` значения присваиваются значения, введенные конечным пользователем в интерфейс правки элемента GridView
2. ObjectDataSource `Update()` метод, выполняя обновление указанной записи
3. GridView повторную привязку к элементу ObjectDataSource, вызвав его `Select()` метод

Значения первичного ключа, назначенные `UpdateParameters` на шаге 1, берутся из значений, указанных в `DataKeyNames` свойство, тогда как вторичного ключа значения берутся из текста в элементы управления TextBox Web для редактируемой строки. Как в случае удаления, крайне важно, элемента GridView `DataKeyNames` установлено правильно. Если он отсутствует, `UpdateParameters` значение первичного ключа будет назначаться `null` значение на шаге 1, что в свою очередь не приведет любой обновленных записей на шаге 2.

Функциональные возможности редактирования может быть активирован, просто установив флажок Enable Editing в смарт-теге элемента GridView.


![Проверка включения редактирования флажок](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Рис. 15**: Проверьте включение, изменение флажок


Проверка флажок Enable Editing добавит поле CommandField (при необходимости) и задайте его `ShowEditButton` свойства `true`. Как было сказано ранее, CommandField содержит ряд `ShowXButton` свойств, которые указывают, какие из набора кнопок изображаются CommandField. Установив флажок Enable Editing добавляет `ShowEditButton` свойства существующих CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Это все, что требуется для добавления элементарного редактирования. Как показано на рисунке 16, интерфейс редактирования довольно каждого типа BoundField которого `ReadOnly` свойству `false` (по умолчанию) подготавливается к просмотру как элемент TextBox. Сюда входят такие поля, как `CategoryID` и `SupplierID`, которые являются ключевыми для других таблиц.


[![Нажав кнопку "Изменить" s Chai отображается строка в режиме редактирования](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Рис. 16**: s Chai, нажав кнопку "Изменить" отображает строку в режиме правки ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


В дополнение к просить пользователей для непосредственного редактирования значения внешнего ключа, интерфейс редактирования хватает одним из следующих способов:

- Если пользователь введет `CategoryID` или `SupplierID` , не существует в базе данных, `UPDATE` нарушают ограничение внешнего ключа, вызывая исключение создано.
- Интерфейс редактирования отсутствует какой-либо проверки. Если не указать требуемое значение (например, `ProductName`), или введите строковое значение, где ожидается числовое значение (например, введя «Слишком много!» в `UnitPrice` текстовое поле), будет вызвано исключение. Следующем учебном курсе будет рассмотрено Добавление элементов управления проверки к интерфейсу редактирования.
- В настоящее время *все* полей продукта, которые не только для чтения должны быть включены в GridView. Если сейчас, чтобы удалить поле из GridView, скажем `UnitPrice`, при обновлении данных GridView не устанавливал бы значение `UnitPrice` `UpdateParameters` значение, которое изменит записи базы данных `UnitPrice` для `NULL` значение. Аналогичным образом, если требуемое поле, такие как `ProductName`, удаляется из GridView, операция обновления заканчивается аварийно с тем же "*столбце «ProductName» запрещены значения NULL*" исключение, упомянутых выше.
- Интерфейс редактирования форматирование оставляет желать много лучшего. `UnitPrice` Показано с четырьмя десятичными. В идеале `CategoryID` и `SupplierID` должны включать элементов управления DropDownList, список категорий и поставщиков в системе.

Всеми этими недостатками, которые нам придется смириться с сейчас, но будет устранена в последующих учебных курсах.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Вставка, редактирование и удаление данных с помощью элемента управления DetailsView

Как мы видели в предыдущих учебных курсах, элемент управления DetailsView отображает по одной записи за раз и, например GridView, предоставляет возможность редактирования и удаления отображаемой записи. Оба пользователя опыт редактирования и удаления элементов в элементе управления DetailsView и рабочий процесс на стороне ASP.NET идентичны данным, элемента управления GridView. Когда DetailsView отличается от GridView — что он также предоставляет встроенную поддержку вставки.

Для демонстрации возможностей изменения данных элемента управления GridView, начните с добавления элемента управления DetailsView для `Basics.aspx` странице над существующей GridView и привяжите его к существующей ObjectDataSource смарт-теге DetailsView. Затем очистите DetailsView `Height` и `Width` свойства и установите флажок Enable Paging в смарт-теге. Чтобы включить редактирование, вставка и удаление поддержки, просто установите флажки Разрешить изменение, включение вставки и разрешить удаление смарт-тега.


![Настройка элемента управления DetailsView для поддержки редактирования, вставки и удаления](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Рис. 17**: Настройка элемента управления DetailsView для поддержки редактирования, вставки и удаления


Как с GridView, добавление редактирования, вставки или удаления поддержки добавлено поле добавляет CommandField к элементу управления DetailsView, как показано в следующем декларативный синтаксис:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Обратите внимание на то, что для элемента DetailsView CommandField отображается в конце коллекции столбцов по умолчанию. Поскольку DetailsView поля отображаются как строки, CommandField отображается как строка с инструкцией Insert, изменять и удалять кнопки в нижней части элемента управления DetailsView.


[![Настройка элемента управления DetailsView для поддержки редактирования, вставки и удаления](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Рис. 18**: Настройка элемента управления DetailsView для поддержки редактирования, вставки и удаления ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


При нажатии кнопки Delete запускает ту же последовательность событий, как и в случае с GridView: обратная передача, следуют DetailsView, заполнение его ObjectDataSource `DeleteParameters` на основе `DataKeyNames` значений; и завершения с помощью вызова его ObjectDataSource `Delete()` метод, который удаляет продукт из базы данных. Также редактирования в элементе DetailsView работает таким образом, идентична GridView.

Для вставки, конечному пользователю предоставляется New кнопка, при нажатии отображает DetailsView в «режиме вставки». В «режиме вставки» "Создать" заменяется кнопки вставки "и" Отмена "и" только эти поля BoundField, кроме которого `InsertVisible` свойству `true` (по умолчанию), отображаются. Поля данных определены как поля автоприращения, такие как `ProductID`, имеют свои [свойство InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) присвоено `false` при привязке к источнику данных через смарт-тег элемента управления DetailsView.

При привязке источника данных в элементе управления DetailsView через смарт-тег, Visual Studio задает `InsertVisible` свойства `false` только для полей автоприращения. Поля, доступные только для чтения, таких как `CategoryName` и `SupplierName`, будет отображаться в пользовательском интерфейсе «режиме вставки», если только их `InsertVisible` явно задано значение `false`. Отвлекитесь и задавать эти два поля `InsertVisible` свойства `false`, либо через декларативный синтаксис DetailsView или изменить поля ссылку в смарт-тег. Рис. 19 показана установка `InsertVisible` свойства `false` по ссылке на изменить поля.


[![Поставщики Northwind теперь поставляют Продукт Acme Tea](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Рис. 19**: Northwind Traders теперь предлагает Acme Tea ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


После задания `InsertVisible` свойства, представление `Basics.aspx` в браузере и нажмите кнопку "Создать". Рис. 20 показан элемент DetailsView, при добавлении нового напитка, Acme Tea, чтобы Линейка наших продуктов.


[![Поставщики Northwind теперь поставляют Продукт Acme Tea](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Рис. 20**: Northwind Traders теперь предлагает Acme Tea ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


После ввода сведений о продукте Acme Tea и нажатия кнопки "Вставить", обратная передача и добавляется новая запись `Products` таблицы базы данных. Так как этот DetailsView перечисляет продукты в порядке, с которой они существуют в таблице базы данных, мы должны перейти к последней странице продукта для просмотра нового продукта.


[![Подробные сведения о продукте Acme Tea](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Рис. 21**: подробные сведения о продукте Acme Tea ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsView [свойство CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) указывает отображение интерфейс и может принимать одно из следующих значений: `Edit`, `Insert`, или `ReadOnly`. [Свойства DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) указывает режим элемента управления DetailsView возврата после правки или вставки завершен и может использоваться для отображения элемент DetailsView, окончательно редактирования или вставки в режиме вставки.


Выберите команды и вставляете или изменяете возможности DetailsView страдал те же ограничения, что GridView: пользователь должен ввести существующие `CategoryID` и `SupplierID` значения в текстовом поле; в интерфейсе отсутствует логики проверки; все поля продукта, которые не допускают `NULL` значения или не иметь значение по умолчанию значение, указанное на уровне базы данных должны быть включены в интерфейс вставки и т. д.

Методы будут рассмотрены для расширения и улучшение GridView интерфейс правки в будущих статьях, которые могут применяться для элемента управления DetailsView интерфейсы правки и вставки также.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Использование FormView для более гибкий интерфейс пользователя изменения данных

FormView предлагает встроенную поддержку для редактирования, вставки и удаления данных, но он использует шаблоны вместо полей есть отсутствует место для добавления полей BoundFields или CommandField, используемые элементами управления GridView и DetailsView для предоставления данных интерфейс изменения. Вместо этого этот интерфейс веб-элементы управления для сбора пользователя входных данных при добавлении нового элемента или редактировании существующего, а также создать, изменить, удалить, вставки, обновления и кнопок отмены необходимо вручную добавить к соответствующим шаблонам. К счастью Visual Studio автоматически создает необходимые элементы интерфейса при привязке FormView к источнику данных с помощью раскрывающегося списка в смарт-тег.

Чтобы продемонстрировать данные методы, сначала добавьте элемент управления FormView для `Basics.aspx` странице и из смарт-тега FormView, привязать его к элементу ObjectDataSource, который уже создан. При этом будут созданы `EditItemTemplate`, `InsertItemTemplate`, и `ItemTemplate` для FormView TextBox веб-элементами управления для сбора входных данных пользователя и кнопку веб-элементы управления для создания, изменение, удаление, вставки, обновления и кнопки отмены. Кроме того, FormView `DataKeyNames` свойству поле первичного ключа (`ProductID`) объектов, возвращаемых элементом ObjectDataSource. Наконец установите флажок Enable Paging в смарт-тега FormView.

Ниже приведена декларативная разметка для элемента FormView `ItemTemplate` после привязки FormView к ObjectDataSource. По умолчанию для всех полей нелогических значений продукта привязан к `Text` свойства элемента управления Label Web при всех полей логических значений (`Discontinued`) привязывается к `Checked` свойство как отключенный элемент управления CheckBox Web. Чтобы активировать определенное поведение FormView при нажатии кнопки New, Edit и Delete, крайне важно, их `CommandName` быть заданы значения для `New`, `Edit`, и `Delete`, соответственно.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Рис. 22 показан FormView `ItemTemplate` при просмотре через браузер. Все поля продуктов отображается с кнопками New, Edit и Delete в нижней.


[![ItemTemplate по умолчанию FormView указаны все поля продуктов вместе с New, изменять и удалять кнопки](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Рис. 22**: по умолчанию FormView `ItemTemplate` перечислены Each продукта поля вместе с New, Edit и удалять кнопки ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Как с помощью GridView и DetailsView, нажав кнопку «Удалить» или любой кнопки, LinkButton или ImageButton которого `CommandName` свойство имеет значение Delete вызывает обратную передачу, заполняет ObjectDataSource `DeleteParameters` основании FormView `DataKeyNames`значение и вызывает ObjectDataSource `Delete()` метод.

При нажатии кнопки Edit обратная передача и привязываются к `EditItemTemplate`, который отвечает за отображение интерфейса редактирования. Этот интерфейс включает веб-элементы управления для редактирования данных, а также кнопки обновления и отмены. Значение по умолчанию `EditItemTemplate` созданные Visual Studio содержит метку для всех полей автоприращения (`ProductID`), текстового поля для всех полей нелогических значений и флажок для всех полей логических значений. Это очень похоже на автоматически сгенерированные поля BoundFields в элементах управления GridView и DetailsView.

> [!NOTE]
> Одной небольшой проблемой FormView автоматическое создание `EditItemTemplate` является обработки TextBox Web для полей, которые доступны только для чтения, таких как `CategoryName` и `SupplierName`. Мы рассмотрим это учесть чуть ниже.


Элементы управления TextBox в `EditItemTemplate` имеют свои `Text` , привязанное к значению, их соответствующие поля данных при помощи *двусторонней привязки данных*. Двусторонняя привязка к данным, обозначенное с помощью `<%# Bind("dataField") %>`, выполняет привязку данных обоих во время привязки данных к шаблону и заполнении параметров ObjectDataSource для вставки и редактирования записей. То есть в том случае, когда пользователь нажимает кнопку редактирования из `ItemTemplate`, `Bind()` метод возвращает значение указанного поля. После того как пользователь производит свои изменения и последнее обновление, отправленные обратно значения, соответствующие полям данных, заданные с помощью `Bind()` применяются к элементу ObjectDataSource `UpdateParameters`. Кроме того, односторонней привязки данных, обозначенное с помощью `<%# Eval("dataField") %>`, только извлекает значения полей данных при привязке данных к шаблону и выполняет *не* возврата значений, введенных пользователем в параметры источника данных при обратной передаче.

В следующей декларативной разметке показан FormView `EditItemTemplate`. Обратите внимание, что `Bind()` метод используется в синтаксис привязки данных и элементы управления, обновление и Отмена Web кнопку иметь их `CommandName` свойства, установите соответствующим образом.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Наши `EditItemTemplate`, это выберите, будут вызывать исключение, если предпринимается попытка использовать его. Проблема в том, что `CategoryName` и `SupplierName` поля подготавливаются к просмотру как элементы управления TextBox Web в `EditItemTemplate`. Необходимо либо изменить эти текстовые поля в метки или полностью удалить их. Давайте просто удалить их из `EditItemTemplate`.

Рис. 23 показан элемент FormView в обозревателе после нажатия "Изменить" для продукта Chai. Обратите внимание, что `SupplierName` и `CategoryName` поля, показанные на `ItemTemplate` , больше нет, так как мы только что были удалены из `EditItemTemplate`. При нажатии кнопки обновления FormView последовательно выполняет все же последовательность действий, как элементы управления GridView и DetailsView.


[![По умолчанию в шаблоне EditItemTemplate все редактируемые поля продуктов как TextBox и CheckBox](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Рис. 23**: по умолчанию `EditItemTemplate` показывает Each редактируемой продукта поле как TextBox и CheckBox ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


При нажатии кнопки вставки FormView `ItemTemplate` обратная. Тем не менее данные не привязан к FormView, так как добавляется новая запись. `InsertItemTemplate` Интерфейс включает веб-элементы управления для добавления новой записи, а также кнопки вставки и "Отмена". Значение по умолчанию `InsertItemTemplate` созданные Visual Studio содержит текстовое поле для всех полей нелогических значений и флажок для всех полей логических значений, аналогичную автоматически созданные `EditItemTemplate`элемента интерфейса. Эти элементы управления TextBox их `Text` , привязанное к значению соответствующего поля данных с помощью двусторонней привязки данных.

В следующей декларативной разметке показан FormView `InsertItemTemplate`. Обратите внимание, что `Bind()` метод используется в синтаксис привязки данных и элементы управления Insert и Отмена Web кнопки имеют свои `CommandName` свойства, установите соответствующим образом.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Имеется один важный нюанс с FormView автоматическое создание `InsertItemTemplate`. В частности, элементы управления TextBox Web создаются даже для полей, доступных только для чтения, таких как `CategoryName` и `SupplierName`. Как с помощью `EditItemTemplate`, нам необходимо удалить эти текстовые поля из `InsertItemTemplate`.

Рис. 24 показан элемент FormView в браузере, при добавлении нового продукта, Acme Coffee. Обратите внимание, что `SupplierName` и `CategoryName` поля, показанные на `ItemTemplate` , больше нет, так как мы только что были удалены. При нажатии кнопки "Вставить" выручка FormView ту же последовательность шагов в элементе управления DetailsView, добавив новую запись для `Products` таблицы. Рис. 25 показаны сведения о продукте Acme Coffee в FormView, после его вставки.


[![Шаблон InsertItemTemplate определяет интерфейс вставки FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Рис. 24**: `InsertItemTemplate` определяет интерфейс вставки FormView ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Подробные сведения о новом продукте, Acme Coffee, отображаются в элеменет FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Рис. 25**: подробные сведения о новом продукте, Acme Coffee, отображаются в элеменет FormView ([Просмотр полноразмерного изображения](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Интерфейсы правки и вставки на три различные шаблона, выделяя только для чтения, FormView позволяет точнее контролировать эти интерфейсы, чем DetailsView и GridView.

> [!NOTE]
> DetailsView, FormView, такие как `CurrentMode` свойство указывает отображение интерфейса и его `DefaultMode` свойство указывает режим FormView возврат после изменения или завершения вставки.


## <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели основные принципы Вставка, редактирование и удаление данных с помощью GridView, DetailsView и FormView. Все эти три элемента управления предоставляют некоторый уровень возможностей изменения данных, которые могут использоваться без написания кода на странице ASP.NET благодаря веб-элементы управления данными и элемент управления ObjectDataSource. Тем не менее простой точки и нажмите кнопку методы отрисовки довольно frail и упрощенный данных изменения пользовательского интерфейса. Для выполнения проверки, добавления программных значений, корректно обрабатывать исключения, настройки пользовательского интерфейса и т. д., нам нужно будет использовать множество методов, которые будут рассмотрены нескольких следующих учебных курсах.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Вперед](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
