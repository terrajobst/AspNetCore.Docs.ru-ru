---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: "Обзор событий, связанных с вставки, обновления и удаления (C#) | Документы Microsoft"
author: rick-anderson
description: "В данном руководстве, мы изучим, с помощью событий, возникающих до, во время и после выполнения вставки обновления или удаления веб-элемента управления данными ASP.NET. W..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 93da23d58d1ba73c5b97f42631d036dd364de24d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Обзор событий, связанных с вставки, обновления и удаления (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) или [скачать PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> В данном руководстве, мы изучим, с помощью событий, возникающих до, во время и после выполнения вставки обновления или удаления веб-элемента управления данными ASP.NET. Также будет показано, как настройка интерфейса правки для обновления только подмножества полей продукта.


## <a name="introduction"></a>Вступление

При использовании встроенных вставки, редактирования или удаления возможности элементов управления GridView, DetailsView и FormView, пройти ряд действий, когда конечный пользователь завершает процесс добавления новой записи или удаления существующей записи. Как уже говорилось в [с предыдущим учебником](an-overview-of-inserting-updating-and-deleting-data-cs.md), при редактировании строки в GridView, кнопка "Изменить" заменяется кнопок "обновления" и "Отмена" и включите стояли в текстовых полях. Последнее обновление конечный пользователь обновляет данные, при обратной передаче выполняются следующие действия:

1. GridView заполняет его ObjectDataSource `UpdateParameters` с измененной записи уникальный идентифицирующий полей (через `DataKeyNames` свойства) и значения, введенные пользователем
2. GridView вызывает его ObjectDataSource `Update()` метод, который в свою очередь вызывает соответствующий метод из базового объекта (`ProductsDAL.UpdateProduct`, в нашем с предыдущим учебником)
3. Базовых данных, которая теперь включает обновленные изменения, повторно привязываются к GridView

Во время этой последовательности этапов ряд событий, позволяющих создать обработчики событий для добавления пользовательской логики при необходимости. Например, до шага 1, GridView `RowUpdating` вызывается событие. Мы на этом этапе можно отменить запрос на обновление, если имеется ошибка проверки. Когда `Update()` вызывается метод, элемент управления ObjectDataSource `Updating` события, предоставляя возможность добавить или настроить значения любого из `UpdateParameters`. После основного ObjectDataSource метода объекта, порождается ObjectDataSource `Updated` события. Обработчик событий для `Updated` может проверить сведений об операции обновления, такие как количество строк, затронутых и ли возникло исключение. Наконец, после шага 2, GridView `RowUpdated` событие, обработчик событий для события, это может рассмотреть дополнительную информацию о точно так же, операция обновления выполняется.

Рис. 1 представлен ряд события и действия при обновлении элемента управления GridView. Модель событий на рисунке 1 не является уникальным для обновления с помощью GridView. Вставка, обновление или удаление данных из GridView, DetailsView и FormView запускает ту же последовательность событий предварительного и последующего уровней для веб-управления данными и ObjectDataSource.


[![Серии до и после события срабатывают при обновлении данных в элементе управления GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Рис. 1**: серии из до и после события срабатывают при обновлении данных в элементе управления GridView ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


В данном руководстве, мы изучим, с помощью этих событий расширить встроенные возможности вставки, обновления и удаления данных ASP.NET возможностей веб-элементов управления. Также будет показано, как настройка интерфейса правки для обновления только подмножества полей продукта.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Шаг 1: Обновление продукта`ProductName`и`UnitPrice`поля

В интерфейсы правки из предыдущего учебника *все* поля продукта, которые не были доступны только для чтения должны быть включены. Если удалить поле из GridView - сказать `QuantityPerUnit` - при обновлении данных веб-управления данными не устанавливал бы ObjectDataSource `QuantityPerUnit` `UpdateParameters` значение. ObjectDataSource передавал бы `null` значение в `UpdateProduct` слой бизнес-логики (BLL) метода, который будет изменять записи измененной базы данных `QuantityPerUnit` столбец `NULL` значение. Аналогично, если обязательное поле, такие как `ProductName`, удаляется из интерфейса правки, обновление завершится ошибкой с «*столбец «ProductName» не допускает значения NULL*"исключение. Причина этого поведения: так как элемент управления ObjectDataSource был настроен для вызова `ProductsBLL` класса `UpdateProduct` метод, который требуется входной параметр для каждого из полей продукта. Таким образом, ObjectDataSource `UpdateParameters` коллекция содержала значение параметра для каждого метода входных параметров.

Если требуется предоставить веб-элемент управления, который позволяет конечным пользователям обновлять только подмножество полей данных, то необходимо либо программно задать отсутствующие `UpdateParameters` значения в ObjectDataSource `Updating` обработчика событий или создать и вызвать метод уровень бизнес-ЛОГИКИ, который ожидает только подмножество полей. Рассмотрим этот метод.

В частности, давайте создадим страница, отображающая только `ProductName` и `UnitPrice` полей в изменяемого элемента управления GridView. Интерфейс редактирования элемента GridView допускает только пользователю о необходимости обновить два отображаемых поля `ProductName` и `UnitPrice`. Поскольку этот интерфейс правки предоставляет только подмножество полей продукта, необходимо либо создать ObjectDataSource, который использует существующего МЕТОДА `UpdateProduct` метод и содержит недостающие значения полей продукта установить в обработчике его `Updating` событий обработчик, или нам нужно создать новый метод уровень бизнес-ЛОГИКИ, который ожидает только подмножество полей, определенных в GridView. В этом учебнике, давайте второй вариант следует использовать и создать перегрузку `UpdateProduct` метод, принимающий только три входных параметра: `productName`, `unitPrice`, и `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Как и исходный `UpdateProduct` метод, эта перегрузка начинает с проверки, существует ли продукт в базе данных с указанным `ProductID`. Если нет, она возвращает `false`, указывающее, что не удалось выполнить запрос, чтобы обновить сведения о продукте. В противном случае он обновляет существующую запись продукта `ProductName` и `UnitPrice` полей соответствующим образом и обновление фиксируется путем вызова TableAdpater `Update()` метод, передавая `ProductsRow` экземпляра.

Сделав это добавление к нашей `ProductsBLL` класса, мы готовы создавать упрощенный интерфейс GridView. Откройте `DataModificationEvents.aspx` в `EditInsertDelete` папку и добавить на страницу элемент управления GridView. Создать новый элемент управления ObjectDataSource и настройте его для использования `ProductsBLL` класса с его `Select()` сопоставление метод `GetProducts` и его `Update()` сопоставление метод `UpdateProduct` перегрузку, которая принимает только `productName`, `unitPrice`, и `productID` входные параметры. На рисунке 2 представлен мастер создания источника данных, при сопоставлении элемента управления ObjectDataSource `Update()` метод `ProductsBLL` для нового класса `UpdateProduct` перегрузки метода.


[![Новая перегрузка метода UpdateProduct сопоставить метод Update() ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Рис. 2**: сопоставление ObjectDataSource `Update()` метод создать `UpdateProduct` перегрузки ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Так как в нашем примере исходно требуется только возможность изменять данные, но не вставлять или удалять записи, пора явно указывать, что элемент управления ObjectDataSource `Insert()` и `Delete()` методы не должны сопоставляться с любой из `ProductsBLL` методы класса, перейдя на вкладках INSERT и DELETE и выбора (нет) из раскрывающегося списка.


[![(Нет) выберите из раскрывающегося списка для вставки и удаления вкладок](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Рис. 3**: параметр (нет) из раскрывающегося списка для вставки и удаления вкладок ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


После завершения работы мастера установите флажок Разрешить изменение смарт-теге элемента GridView.

С помощью мастера создания источника данных и привязки, к GridView завершение создаваемые в Visual Studio декларативный синтаксис для обоих элементов управления. Перейдите к представлению источника для проверки элемента управления ObjectDataSource декларативная разметка, как показано ниже:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Поскольку нет сопоставлений для элемента управления ObjectDataSource `Insert()` и `Delete()` методов, существуют не `InsertParameters` или `DeleteParameters` разделы. Кроме того, с момента `Update()` метода сопоставлен `UpdateProduct` перегрузки метода, который принимает три входных параметра только `UpdateParameters` присутствуют только три экземпляра `Parameter` экземпляров.

Обратите внимание, что элемент управления ObjectDataSource `OldValuesParameterFormatString` свойству `original_{0}`. Это свойство задается с помощью Visual Studio автоматически, при использовании мастера настройки источника данных. Тем не менее поскольку наши методы уровень бизнес-ЛОГИКИ не ожидается, что исходный `ProductID` значение для передачи, полностью удалить назначение этого свойства элемента управления ObjectDataSource декларативного синтаксиса.

> [!NOTE]
> Если просто очистить `OldValuesParameterFormatString` значение свойства в окне свойств в режиме конструктора, свойство по-прежнему будет существовать в декларативного синтаксиса, но устанавливается равным пустой строке. Либо удалите полностью из декларативного синтаксиса, или в окне свойств свойство значение по умолчанию `{0}`.


Хотя элемент управления ObjectDataSource имеет только `UpdateParameters` для имени продукта, цену и идентификатор, среда Visual Studio добавила BoundField или CheckBoxField в GridView для каждого из полей продукта.


[![Содержит GridView BoundField или CheckBoxField для каждого из полей продукта](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Рис. 4**: содержит GridView BoundField или CheckBoxField для каждого из полей продукта ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Если конечный пользователь редактирует продукта и нажатия кнопки его обновление GridView перечисляются эти поля, которые не были доступны только для чтения. Затем устанавливает значение соответствующего параметра в коллекции `UpdateParameters` коллекции до значения, введенные пользователем. Если не создается соответствующий параметр, GridView добавляет ее в коллекцию. Таким образом, если наш GridView содержит стояли и CheckBoxFields для всех полей продукта, ObjectDataSource вызовет перегрузку `UpdateProduct` , принимающую все эти параметры, несмотря на то, что элемент управления ObjectDataSource декларативная разметка указывает только три входных параметра (см. рис. 5). Аналогично, если сочетание не — только для чтения продукта полей в GridView, не соответствует входные параметры для `UpdateProduct` перегрузки, будет вызвано исключение при попытке обновления.


[![GridView будет добавлять параметры для элемента управления ObjectDataSource UpdateParameters коллекции](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Рис. 5**: GridView будет добавить параметры в элемент управления ObjectDataSource `UpdateParameters` коллекции ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Чтобы убедиться, что вызывает элемент управления ObjectDataSource `UpdateProduct` , принимающую только продукта имя, цену и идентификатор, нам нужно ограничить GridView полями для только что `ProductName` и `UnitPrice`. Это можно сделать, удалив стояли и CheckBoxFields, задав те других полей `ReadOnly` свойства `true`, или с помощью некоторого сочетания двух. В этом учебнике Давайте просто удалите все поля GridView, кроме `ProductName` и `UnitPrice` стояли, после чего GridView будет выглядеть как:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Несмотря на то что `UpdateProduct` рассчитывает на три входных параметра, у нас имеется только два стояли в нашем GridView. Это вызвано `productID` входной параметр имеет значение первичного ключа и передать значение `DataKeyNames` свойства для измененной строки.

Наш GridView, вместе с `UpdateProduct` перегрузки дает пользователю возможность изменять только имя и цену продукта без потери каких-либо другие поля продукта.


[![Интерфейс позволяет редактировать только имя и цену продукта](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Рис. 6**: позволяет интерфейса, изменение только имя и цену продукта ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Как описано в предыдущем учебнике жизненно важна, GridView, быть s состояния просмотра включена (по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, увеличивается риск того что параллельно работающие пользователи непреднамеренно удаление или изменение записей. В разделе [предупреждение: проблемы параллелизма с ASP.NET 2.0 GridViews/DetailsView/FormViews, поддерживают редактирование или удаление и с состояние просмотра отключено](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) для получения дополнительной информации.


## <a name="improving-theunitpriceformatting"></a>Повышение`UnitPrice`форматирование

Пример GridView, показано на рис. 6 работает, а `UnitPrice` поле вообще не отформатирован, что приводит к отображению цены не обеспечивает валют, символов и имеет четыре десятичных разряда. Чтобы применить форматирование для неизменяемых строк денежных единиц, просто установите `UnitPrice` BoundField `DataFormatString` свойства `{0:c}` и его `HtmlEncode` свойства `false`.


[![Задание свойств HtmlEncode и DataFormatString UnitPrice соответствующим образом](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Рис. 7**: задать `UnitPrice` `DataFormatString` и `HtmlEncode` свойства соответствующим образом ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Благодаря этому изменению нередактируемые строк цена форматируется как денежное значение; Тем не менее, редактируемой строки по-прежнему отображается значение без знака денежной единицы и четыре десятичных разряда.


[![Нередактируемые строки являются отформатирован в виде значения валюты](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Рис. 8**: неизменяемые строки, отформатирован в виде значения валюты ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Указанный в инструкции форматирования `DataFormatString` свойства могут применяться к интерфейсу редактирования установим для `ApplyFormatInEditMode` свойства `true` (значение по умолчанию — `false`). Теперь пора присвоить этому свойству значение `true`.


[![UnitPrice BoundField ApplyFormatInEditMode свойство имеет значение true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Рис. 9**: задать `UnitPrice` BoundField `ApplyFormatInEditMode` свойства `true` ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Благодаря этому изменению значение `UnitPrice` отображаются в измененной строки также форматируются как денежное значение.


[![Значение UnitPrice строки редактирования — отформатирован в виде валюты](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Рис. 10**: изменить строки `UnitPrice` значение — отформатирован в виде валюты ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Однако обновление продукта с символом денежной единицы в текстовом поле, такие как $19,00 вызывает `FormatException`. При попытке назначить введенных пользователем значений элемента управления ObjectDataSource GridView `UpdateParameters` коллекции, его не удается преобразовать `UnitPrice` строку «$19,00» в `decimal` требует параметр (см. рис. 11). Чтобы устранить эту можно создать обработчик событий для элемента GridView `RowUpdating` событие и его синтаксический анализ, предоставленное пользователем `UnitPrice` в формате валюты `decimal`.

GridView `RowUpdating` событий принимает в качестве второго параметра объект типа [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), которая содержит `NewValues` в качестве одного из его свойств введенных пользователем значений готовые для назначенные ObjectDataSource `UpdateParameters` коллекции. Мы существующее значение `UnitPrice` значение в `NewValues` синтаксический анализ коллекции с десятичным значением в формате валюты с помощью следующего фрагмента кода в `RowUpdating` обработчик событий:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Если пользователь предоставил `UnitPrice` значение (например, «$19,00»), это значение перезаписывается десятичное значение, вычисленное по [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), синтаксический анализ как денежное значение. Это будет разбирать десятичного разделителя в случае символы валют, запятые, десятичного разделителя и т. д и использует [перечисления NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) в [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) пространства имен.

Рис. 11 показана проблемы, вызванные символы валют в предоставленный пользователем `UnitPrice`, а также как GridView `RowUpdating` обработчик событий можно использовать для анализа таких входных данных правильно.


[![Значение UnitPrice строки редактирования — отформатирован в виде валюты](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Рис. 11**: изменить строки `UnitPrice` значение — отформатирован в виде валюты ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Шаг 2: Запрет`NULL UnitPrices`

Пока база данных настроена так, чтобы разрешить `NULL` значения в `Products` таблицы `UnitPrice` столбец, нужно запретить пользователям, посетив страницу из указание `NULL` `UnitPrice` значение. То есть если пользователь не ввел `UnitPrice` значение при редактировании строку продукта, а не сохранить результаты в базу данных, необходимо отобразить сообщение о том, что на данной странице всех изменяемых продуктов необходимо указывать цену.

`GridViewUpdateEventArgs` , Переданное в GridView `RowUpdating` обработчик события содержит `Cancel` свойства, если значение `true`, завершает процесс обновления. Расширим `RowUpdating` обработчик событий, чтобы задать `e.Cancel` для `true` и отображать сообщение о том, почему при `UnitPrice` значение в `NewValues` коллекция `null`.

Начните с добавления веб-управления Label на страницу с именем `MustProvideUnitPriceMessage`. Этот элемент управления Label будет отображаться, если пользователь не указал `UnitPrice` значение при обновлении продукта. Задайте для свойства Label `Text` свойства «Необходимо указать цену продукта.» Также создали класс CSS в `Styles.css` с именем `Warning` со следующим определением:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Наконец, задайте для свойства Label `CssClass` свойства `Warning`. В этот момент конструктор должен вывести сообщение с предупреждением в красный, полужирный, курсив, очень большой размер выше GridView, как показано на рис. 12.


[![Метка будет добавлен над элементом управления GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Рис. 12**: метка имеет были добавлены выше GridView ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


По умолчанию эта метка должен быть скрыт, поэтому установите его `Visible` свойства `false` в `Page_Load` обработчик событий:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Если пользователь пытается обновить продукт без указания `UnitPrice`, нам нужно отменить обновление и отобразить предупреждение. Дополнять GridView `RowUpdating` обработчик событий следующим образом:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Если пользователь пытается сохранить продукт без указания цены, обновление отменяется, и отображается информационное сообщение. Хотя базы данных (и бизнес-логики) позволяет `NULL` `UnitPrice` s, эта конкретная страница ASP.NET — нет.


[![Пользователь нельзя оставлять пустым UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Рис. 13**: пользователь не может быть передано `UnitPrice` пустой ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Пока мы рассмотрели использование GridView `RowUpdating` событий для программного изменения значений параметров, назначенные ObjectDataSource `UpdateParameters` коллекции также как для отмены процедуры обновления полностью. Эти концепции переносятся на элементы управления DetailsView и FormView и справедливы и для вставки и удаления.

Эти задачи можно также на уровне ObjectDataSource с помощью обработчиков событий для его `Inserting`, `Updating`, и `Deleting` события. Эти события срабатывают до вызова соответствующего метода базового объекта и укажите последнюю возможность изменить коллекцию входных параметров или отменить операцию. Обработчики событий для этих трех событий передаются в объект типа [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , имеет два свойства, представляющие интерес:

- [Отмена](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), который, если значение `true`, отмена выполняемой операции.
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), которое представляет собой коллекцию из `InsertParameters`, `UpdateParameters`, или `DeleteParameters`в зависимости от того, является ли обработчик событий для `Inserting`, `Updating`, или `Deleting` событий

Чтобы продемонстрировать работу со значениями параметров на уровне ObjectDataSource, включим DetailsView в нашу страницу, которая позволяет пользователям добавлять новый продукт. Это DetailsView будет использоваться для обеспечения интерфейса для быстрого добавления нового продукта в базу данных. При добавлении нового продукта позволяет разрешить пользователю только введите значения для следует согласованного пользовательского интерфейса `ProductName` и `UnitPrice` поля. По умолчанию эти значения, которые не предоставляются в интерфейсе вставки DetailsView будет присвоено `NULL` значение базы данных. Тем не менее, можно использовать элемент управления ObjectDataSource `Inserting` событие, чтобы ввести другие значения по умолчанию, как мы рассмотрим чуть ниже.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Шаг 3: Создание интерфейса для добавления новых продуктов

Перетащите DetailsView из области элементов в конструктор выше GridView, очистить его `Height` и `Width` свойств и связывание его в элемент управления ObjectDataSource, имеющиеся на странице. Это добавит BoundField или CheckBoxField для каждого из полей продукта. Поскольку мы хотим использовать этот DetailsView для добавления новых продуктов, нам нужно из смарт-тега; установите флажок Разрешить вставку Однако нет такого параметра нет, так как элемент управления ObjectDataSource `Insert()` метода не сопоставлен метод в `ProductsBLL` класса (Напомним, что это сопоставление (нет) задается при настройке источника данных см. рис. 3).

Чтобы настроить элемент управления ObjectDataSource, выберите эту ссылку настройки источника данных в его смарт-теге, запуск мастера. Первый экран позволяет изменять базовый объект, с которым связан элемент управления ObjectDataSource; Оставьте для `ProductsBLL`. На следующем экране перечисляются сопоставления с помощью методов элемента управления ObjectDataSource к базовому объекту. Несмотря на то, что мы явно указано, что `Insert()` и `Delete()` методы не должны сопоставляться ко всем методам, если перейти на вкладках INSERT и DELETE вы увидите, что там находится сопоставление. Это вызвано `ProductsBLL` `AddProduct` и `DeleteProduct` методы используют `DataObjectMethodAttribute` атрибут, чтобы указать, что они являются методами по умолчанию для `Insert()` и `Delete()`соответственно. Таким образом мастер ObjectDataSource выбирает эти каждый раз при запуске мастера при отсутствии явно указано другое значение.

Оставить `Insert()` метод, указывающий `AddProduct` метода, но снова установить раскрывающемся списке вкладки DELETE (нет).


[![Метод AddProduct значение вкладки «Вставка» раскрывающегося списка](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Рис. 14**: значение раскрывающегося списка вкладку Вставка `AddProduct` метод ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Значение в раскрывающемся списке вкладки DELETE (нет)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Рис. 15**: задать удаление вкладки в раскрывающемся списке (нет) ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


После внесения этих изменений, ObjectDataSource декларативный синтаксис будет расширен для включения `InsertParameters` коллекции, как показано ниже:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Повторный запуск мастера вернулось `OldValuesParameterFormatString` свойство. Теперь пора удалите это свойство, присвоив ему значение по умолчанию (`{0}`) либо совсем удалив его из декларативного синтаксиса.

Использование ObjectDataSource, предоставляя возможности вставки смарт-тег DetailsView теперь включают флажок Разрешить вставку; Вернитесь в конструктор и установите этот флажок. Затем сократите DetailsView, придав ему стояли две - `ProductName` и `UnitPrice` - и CommandField. На этом этапе DetailsView должен выглядеть как:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

На рисунке 16 показано эту страницу при просмотре через браузер на этом этапе. Как видите, DetailsView указаны имя и цену первого продукта (Chai). Тем не менее, мы хотим является вставка интерфейс, который предоставляет средства для пользователя для быстрого добавления нового продукта в базу данных.


[![DetailsView является в настоящее время к просмотру в режиме только для чтения](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**На рисунке 16**: DetailsView является в настоящее время к просмотру в режиме только для чтения ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Чтобы можно было отобразить DetailsView в режиме вставки нам нужно указать `DefaultMode` свойства `Inserting`. Подготавливает к просмотру в режиме вставки при первом посещении DetailsView. при этом остается после вставки новой записи. Как показано на рисунке 17, такие DetailsView предоставляет быстрый интерфейс для добавления новой записи.


[![DetailsView предоставляет интерфейс для быстрого добавления нового продукта](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Рисунок 17**: DetailsView предоставляет интерфейс для быстрого добавления нового продукта ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Когда пользователь вводит имя и цену продукта (например, «Acme Water» и 1,99, как показано на рис. 17) и выбирает Insert, обратная передача и запускает поток операций по вставке одновременной в новую запись о продукте добавлены в базу данных. DetailsView сохраняет свой интерфейс для вставки и GridView автоматически повторно привязывается к источнику данных для включения нового продукта, как показано на рисунке 18.


![Продукт](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**На рисунке 18**: продукта «Acme Water» был добавлен в базу данных


Хотя на рисунке 18 GridView не показывает, полей продукта отсутствует в интерфейсе DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`и так далее назначаются `NULL` значения базы данных. Это можно увидеть, выполнив следующие действия:

1. Перейдите в обозреватель серверов в Visual Studio
2. Расширение `NORTHWND.MDF` узел базы данных
3. Щелкните правой кнопкой мыши `Products` базы данных
4. Выберите Показать таблицу данных

Будет выведен список всех записей в `Products` таблицы. Как показано на рис все столбцы наш новый продукт отличный от `ProductID`, `ProductName`, и `UnitPrice` имеют `NULL` значения.


[![Поля продукта не указано в DetailsView будут назначены значения NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**На рисунке 19**: продукта поля не представленным в DetailsView назначаются `NULL` значения ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Необходимо указать значение по умолчанию, отличный от `NULL` для одного или нескольких из этих значений в столбце, либо потому что `NULL` не лучшим вариантом по умолчанию или потому, что сам столбец базы данных не допускает `NULL` s. Для этого мы программно задать значения параметров DetailsView `InputParameters` коллекции. Это назначение можно сделать либо в случае обработчик для элемента DetailsView `ItemInserting` событий или ObjectDataSource `Inserting` событий. Так как мы уже узнали во с помощью событий предварительного и последующего уровней данных Web контролировать уровень, давайте рассмотрим с помощью элемента управления ObjectDataSource событий это время.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Шаг 4: Присвоения значений для`CategoryID`и`SupplierID`параметров

Для этого примера предположим, что для нашего приложения при добавлении нового продукта через этот интерфейс ей должно быть присвоено `CategoryID` и `SupplierID` значение 1. Как упоминалось ранее, ObjectDataSource имеется пара событий предварительного и последующего уровней которые запускаются во время модификации данных. При его `Insert()` вызова метода, элемент управления ObjectDataSource сначала вызывает его `Inserting` , затем вызывает метод, его `Insert()` метод был сопоставлен и наконец создает `Inserted` событий. `Inserting` Дает нам последнюю возможность настроить входные параметры или отменить операцию.

> [!NOTE]
> В реальном приложении, скорее всего вы захотите разрешить пользователю указать категорию и поставщика или это значение будет следует выбирать для них зависимости от некоторых условий или бизнес логики (а не вслепую выбирать идентификатор 1). Независимо от того пример показывает, как программно задать значение входного параметра из предыдущего уровня события элемента управления ObjectDataSource.


Теперь пора создайте обработчик событий для элемента управления ObjectDataSource `Inserting` событий. Обратите внимание, что обработчик событий второй входной параметр объекта типа `ObjectDataSourceMethodEventArgs`, который имеет свойство для доступа к коллекции параметров (`InputParameters`) и свойство, чтобы отменить операцию (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

На этом этапе `InputParameters` свойство содержит элемент управления ObjectDataSource `InsertParameters` коллекции со значениями, присвоенными из DetailsView. Чтобы изменить значение одного из этих параметров, используйте: `e.InputParameters["paramName"] = value`. Таким образом Чтобы задать `CategoryID` и `SupplierID` для значения 1, настройте `Inserting` обработчик событий, чтобы выглядеть следующим образом:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Это время при добавлении нового продукта (например, Acme Soda) `CategoryID` и `SupplierID` столбцов нового продукта устанавливаются в 1 (см. рис. 20).


[![Новые продукты теперь имеют свои CategoryID и SupplierID данные, установленные на 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Рис. 20**: новые продукты теперь имеют их `CategoryID` и `SupplierID` значения равным 1 ([Просмотр полноразмерное изображение](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Сводка

Во время редактирования, вставки и удаления процесса веб-управления данными и ObjectDataSource Пройдите ряд событий предварительного и последующего уровней. В этом учебнике мы рассмотрели предварительного события и узнали, как их использовать для настройки параметров ввода или отменить операцию изменения данных полностью, оба из веб-управления и события для элемента управления ObjectDataSource. В следующем уроке мы рассмотрим создание и использование обработчиков событий для событий последующего уровня.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Jackie Goor и (Liz Shulok). Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](an-overview-of-inserting-updating-and-deleting-data-cs.md)
[Вперед](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
