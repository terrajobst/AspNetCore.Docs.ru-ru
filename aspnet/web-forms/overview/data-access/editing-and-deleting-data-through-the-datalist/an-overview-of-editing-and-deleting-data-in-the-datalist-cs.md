---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Обзор изменения и удаления данных в DataList (C#) | Документы Microsoft
author: rick-anderson
description: Когда элемент управления DataList нет встроенной редактирование и удаление возможностей, в этом учебнике будет показано, как создать DataList, который поддерживает изменение и удаление o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: be86707980b11453ef78fdbddead73ab9808b54d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Обзор изменения и удаления данных в DataList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) или [скачать PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Когда элемент управления DataList нет встроенной редактирование и удаление возможностей, в этом учебнике будет показано, как создать DataList, который поддерживает изменение и удаление из его базовых данных.


## <a name="introduction"></a>Вступление

В [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) учебника мы рассмотрели вставки, обновления и удаления данных с помощью архитектуры приложения, ObjectDataSource GridView, DetailsView и FormView элементы управления. ObjectDataSource и эти три данных веб-элементы управления реализация интерфейсов модификации данных простого было просто и участвуют просто отсчет флажок из смарт-тега. Без кода, необходимого для записи.

К сожалению DataList не имеет встроенной, редактирование и удаление возможности, обеспечиваемые в элементе управления GridView. Эта функция отсутствует из-за частично тот факт, что DataList relic из предыдущей версии ASP.NET, если декларативный источников данных и страниц данных кода без изменения были недоступны. Пока DataList в ASP.NET 2.0 предлагает таким же, без дополнительной настройки возможности модификации данных как GridView, мы используем ASP.NET 1.x методы для включения такой функции. Этот подход требует большой объем кода, но, как будет показано в этом учебнике, DataList имеет некоторые события и свойства на месте для упрощения этого процесса.

В этом учебнике мы рассмотрим создание DataList, который поддерживает изменение и удаление из его базовых данных. Будущих учебниках проверит расширенного редактирования и удаление сценариев, включая поля ввода проверки, правильно обработки исключений, созданных из доступа к данным или уровни бизнес-логики и т. д.

> [!NOTE]
> Как элемент управления DataList элементе управления повторителем не имеет внешней функции поле для вставки, обновления или удаления. Хотя можно добавить такие функциональные возможности, DataList включает свойства и события, не найден в Повторителе, которые упрощают добавление таких возможностей. Таким образом этот учебник и последующие, которые отслеживают изменения и удаления будет уделяться строго DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Шаг 1: Создание, редактирование и удаление учебники веб-страниц

Прежде чем начать изучение как обновление и удаление данных из DataList, позволяют s сначала занять некоторое время для создания страниц ASP.NET в данном проекте веб-сайта, он понадобится для этого учебника и далее несколько из них. Начните с добавления в новую папку с именем `EditDeleteDataList`. Добавьте следующие страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Добавление страницы ASP.NET учебников](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Рис. 1**: Добавление страницы ASP.NET учебников


Как и в других папках, `Default.aspx` в `EditDeleteDataList` папки перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Таким образом, добавьте этот пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s представление конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**На рисунке 2**: добавление `SectionLevelTutorialListing.ascx` пользовательского элемента управления на `Default.aspx` ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Наконец, добавления страниц, как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после главного и подчиненного представлений отчетов с DataList и повторителя `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элементы для DataList, редактирование и удаление учебники.


![Карта сайта теперь содержит записи для DataList, редактирование и удаление учебники](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Рис. 3**: карта сайта теперь содержит записи для DataList, редактирование и удаление учебники


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Шаг 2: Проверка методы для обновление и удаление данных

Изменение и удаление данных в GridView так просто, так как в системе, GridView и ObjectDataSource работы в сочетании. Как было сказано в [проверки события, связанные с вставки, обновления и удаления](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) учебника при нажатии кнопки обновления строки s GridView автоматически назначает ее полей, которые двухсторонней привязкой данных к `UpdateParameters` коллекцию методы, а затем вызывает ObjectDataSource s `Update()` метод.

К сожалению DataList не поддерживает любой из этих встроенных функциональных возможностей. Наша ответственность, чтобы убедиться, что пользователь s значения присваиваются параметры s ObjectDataSource и его `Update()` вызывается метод. Чтобы помочь нам в эту задачу, DataList предоставляет следующие свойства и события.

- **[ `DataKeyField` Свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  при обновлении или удалении, мы должны быть способны уникально идентифицировать каждого элемента в DataList. Присвойте этому свойству значение поля первичного ключа отображаемых данных. Таким образом будет заполнять DataList s [ `DataKeys` коллекции](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) с указанным `DataKeyField` значение для каждого элемента DataList.
- **[ `EditCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  возникает, когда кнопку, LinkButton или ImageButton которого `CommandName` свойству нажатии редактирования.
- **[ `CancelCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  возникает, когда кнопку, LinkButton или ImageButton которого `CommandName` свойству нажатии кнопки отмены.
- **[ `UpdateCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  возникает, когда кнопку, LinkButton или ImageButton которого `CommandName` свойству щелчке обновления.
- **[ `DeleteCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  возникает, когда кнопку, LinkButton или ImageButton которого `CommandName` свойству нажатии Delete.

С помощью этих свойств и событий, существует четыре подхода, который можно использовать для обновления и удаления данных из DataList.

1. **С помощью методов ASP.NET 1.x** DataList существовали до ASP.NET 2.0 и ObjectDataSources и был в состоянии обновления и удаления данных полностью с помощью программных средств. Такой подход полностью ditches ObjectDataSource и требует, что привязка данных к уже DataList непосредственно из уровня бизнес-логики, и при получении данных для отображения и при обновлении или удалении записи.
2. **С помощью одного элемента управления ObjectDataSource на странице для выбор, обновления и удаления** когда DataList нет встроенного редактирования GridView s и удаление возможностей, отсутствует s нет необходимости, мы можем t добавьте их в сами. В этом случае мы так же, как использовать элемент управления ObjectDataSource в примерах GridView, но необходимо создать обработчик событий для DataList s `UpdateCommand` событий, где задается параметры s ObjectDataSource и вызовите его `Update()` метод.
3. **С помощью элемента управления ObjectDataSource выбор, но обновление и удаление непосредственно от МЕТОДА** при использовании параметра 2, нам нужно написать небольшой кода в `UpdateCommand` событий, присвоение значения параметров и т. д. Вместо этого мы можно использовать элемент управления ObjectDataSource для выбора, но необходимо обновление и удаление вызовы непосредственно для МЕТОДА (например, с параметром 1). На мой взгляд, обновление данных, работая с МЕТОДА приводит к код более читаемым, чем назначение ObjectDataSource s `UpdateParameters` и вызов его `Update()` метод.
4. **С помощью декларативным образом через несколько ObjectDataSources** предыдущих все три подхода требуется большой объем кода. Если d вместо сохранять используется как гораздо декларативного синтаксиса, насколько это возможно, последний параметр является включение нескольких ObjectDataSources на странице. Первый элемент управления ObjectDataSource получает данные из МЕТОДА и привязывает его к элементу управления DataList. Для обновления, другой элемент управления ObjectDataSource добавлен, но добавить непосредственно в DataList s `EditItemTemplate`. Чтобы включить удаление поддержки, еще другой ObjectDataSource будут требоваться в `ItemTemplate`. При таком подходе эти внедренные использует ObjectDataSource `ControlParameters` для декларативной привязки параметров s ObjectDataSource для пользовательских элементов управления ввода (вместо того, чтобы задать их программным способом в DataList s `UpdateCommand` обработчика событий). Такой подход по-прежнему требуется большой объем кода, нам потребуется вызвать внедренный элемент управления ObjectDataSource s `Update()` или `Delete()` команды, но требует гораздо меньше, чем с другими три подходы. Недостатком здесь является то, что несколько ObjectDataSources загромождать странице отвлекать от общую читаемость.

Если принудительно использовать только один из следующих подходов, я d выберите параметр 1, так как он обеспечивает наибольшую гибкость, а DataList первоначально предназначалась для размещения этого шаблона. Хотя DataList была расширена для работы с элементами управления источника данных ASP.NET 2.0, он не имеет всех точек расширения или функции официальный веб-элементов управления (GridView, DetailsView и FormView) данных ASP.NET 2.0. Параметры 2 по 4, без эффективность, хотя.

Это будущих редактирование и удаление учебники будет использовать ObjectDataSource для получения данных для отображения и направить вызовы МЕТОДА обновления и удаления данных (вариант 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Шаг 3: Добавление DataList и настройка его ObjectDataSource

В этом учебнике мы создадим DataList, который содержит сведения о продукте и для каждого продукта предоставляет пользователю возможность изменить имя и цену и полностью удалить продукт. В частности мы получить записи для отображения с помощью ObjectDataSource, но выполнить обновление и удаление действий, работая с МЕТОДА. Прежде чем мы беспокоиться о реализации возможности редактирования и удаления в DataList, позволяют s сначала перейти на страницу для отображения продуктов в интерфейсе только для чтения. Поскольку мы хранять проверяются следующие действия в предыдущих учебниках я будет выполняться через них быстро.

Сначала откройте `Basics.aspx` страницы в `EditDeleteDataList` папку и добавьте DataList на страницу в режиме конструктора. В смарт-теге DataList s, создайте новый элемент управления ObjectDataSource. Поскольку мы работаем с данными о продуктах, настройте его для использования `ProductsBLL` класса. Для получения *все* выберите продукты, `GetProducts()` метод на вкладке "SELECT".


[![Настройка ObjectDataSource с помощью класса ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Рис. 4**: Настройка ObjectDataSource для использования `ProductsBLL` класса ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Возвращает сведения о продукте, с помощью метода GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Рис. 5**: возвращать сведения о продукта с помощью `GetProducts()` метод ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


Элемент управления DataList, такие как GridView, не предназначена для вставки новых данных; Таким образом, выберите вариант (нет) из раскрывающегося списка на вкладке «Вставка». Также выберите (нет) для вкладок UPDATE и DELETE с момента обновления и удаления будут выполняться программным способом с помощью МЕТОДА.


[![Убедиться, что значение раскрывающихся списков в ObjectDataSource s вставки, обновления и удаления вкладок (нет)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Рис. 6**: Убедитесь, что раскрывающемся списке указаны в ObjectDataSource s INSERT, UPDATE и удаление вкладок заданы (нет) ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


После настройки ObjectDataSource, нажмите кнопку Готово, возвращая в конструктор. Как мы хранять показано в предыдущих примерах, после завершения настройки ObjectDataSource, Visual Studio автоматически создает `ItemTemplate` DropDownList отображение каждого из полей данных. Введите здесь `ItemTemplate` на тот, который отображает название продукта s и цену. Кроме того, задать `RepeatColumns` равным 2.

> [!NOTE]
> Как было сказано в *Обзор Вставка, обновление и удаление данных* учебник, при изменении данных с помощью наших архитектура требует мы удаляем ObjectDataSource `OldValuesParameterFormatString` свойство из ObjectDataSource s декларативная разметка (или сбросить его значение по умолчанию `{0}`). В этом учебнике тем не менее, мы используем ObjectDataSource только для извлечения данных. Таким образом, нам не нужно изменять ObjectDataSource s `OldValuesParameterFormatString` значение свойства (хотя он повредить для этого).


После замены по умолчанию DataList `ItemTemplate` на настроенный, на странице должна выглядеть следующим образом:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Теперь пора просмотрите ход работы через браузер. Как показано на рис. 7, элемент управления DataList отображает продукта название и цену за единицу для каждого продукта в двух столбцах.


[![Названия продуктов и цены отображаются в DataList два столбца](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Рис. 7**: имена продуктов и цен, отображаются в DataList двух столбцов ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> DataList обладает рядом свойств, которые требуются для процесса обновления и удаления, и эти значения сохраняются в состоянии представления. Таким образом при создании DataList, который поддерживает изменение или удаление данных, важно включить состояние просмотра s DataList.  
>   
>  Проницательный читатель вспомнить, что мы смогли отключить состояние просмотра, при создании редактируемой GridViews DetailsViews и FormViews. Это так, как элементы управления ASP.NET 2.0 Web могут содержать *управления состоянием*, который сохраняется во время обратной передачи, например состояние представления, но если принято решение основные состояния.


Отключение просмотра состояния в GridView, просто пропускает сведения о состоянии тривиальный, но сохраняет состояние элемента управления (который включает состояние, необходимое для изменения и удаления). DataList, созданный на период времени 1.x ASP.NET не использует состояние элемента управления и поэтому должно быть включено состояние представления. В разделе [vs состояние элемента управления. Состояние представления](https://msdn.microsoft.com/library/1whwt1k7.aspx) Дополнительные сведения о назначении состояние элемента управления и его отличий от состояния просмотра.

## <a name="step-4-adding-an-editing-user-interface"></a>Шаг 4: Добавление пользовательский интерфейс редактирования

Элемент управления GridView, состоящие из коллекции полей (стояли, CheckBoxFields, TemplateFields и т. д.). Эти поля можно настроить их отображения разметки в зависимости от их режима. Например в режиме только для чтения поле BoundField отображает его значение поля как текст; в режиме редактирования, но отображает Web текстовое свойство `Text` присваивается значение поля данных.

Элемент управления DataList с другой стороны, отображает его элементы, с помощью шаблонов. Элементы, предназначенные только для чтения, отображаются с использованием `ItemTemplate` в то время как элементы в режиме редактирования подготавливаются к просмотру через `EditItemTemplate`. На этом этапе нашей DataList имеет только `ItemTemplate`. Для поддержки функциональных возможностей редактирования уровня элемента, необходимо добавить `EditItemTemplate` , содержащая разметку, отображаемую для изменения элемент. В этом учебнике мы будем использовать текстовое поле веб-элементы управления для редактирования продукта s название и цену за единицу.

`EditItemTemplate` Можно создать декларативно или с помощью конструктора (путем выбора параметра редактирование шаблонов смарт-теге DataList s). Чтобы использовать параметр редактирование шаблонов, сначала щелкните ссылку Изменить шаблоны смарт-тега, а затем выберите `EditItemTemplate` элемент из раскрывающегося списка.


[![Необязательно для работы с DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Рис. 8**: необязательно для работы с DataList s `EditItemTemplate` ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


После этого введите название продукта: и Price: и перетащите два элемента управления TextBox из области элементов в `EditItemTemplate` интерфейс в конструкторе. Набор текстовых полей `ID` свойства `ProductName` и `UnitPrice`.


[![Текстовое поле для имени продукта s и цены](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Рис. 9**: Добавьте текстовое поле имя продукта и Price ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


Нам нужно привязать соответствующими значениями полей данных продукта для `Text` свойства двух текстовых полей. Смарт-тегами текстовые поля, щелкните ссылку изменить привязки данных и затем связать поле с соответствующими данными `Text` свойства, как показано на рис. 10.

> [!NOTE]
> При привязке `UnitPrice` поля данных цену TextBox s `Text` поля, вы может отформатировать их в виде значения валюты (`{0:C}`), число с общим форматом (`{0:N}`), или оставить неформатированный.


![Связать ProductName и полей данных UnitPrice свойства Text текстовых полей](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Рис. 10**: привязка `ProductName` и `UnitPrice` поля данных для `Text` свойств текстовых полей


Обратите внимание на то, как работает окно изменить привязки данных на рис. 10 *не* включают флажок двусторонняя привязка к данным, представленным при редактировании TemplateField в GridView или DetailsView или шаблона в FormView. Функция двухсторонней привязкой данных допустимое значение, введенное в элемент управления Web автоматически назначен соответствующий s ObjectDataSource `InsertParameters` или `UpdateParameters` при вставке или обновлении данных. DataList не поддерживает двусторонняя привязка к данным, как будет показано позднее в этом учебнике после пользователь вносит ей изменения и готов к обновлению данных, вам потребуется получить программный доступ к эти текстовые поля `Text` свойства и передайте их значения для соответствующие `UpdateProduct` метод `ProductsBLL` класса.

Наконец, нужно добавить обновления и «Отмена», чтобы `EditItemTemplate`. Как мы видели в [главный/Detail, с помощью маркированного списка из главного записей с DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) учебник, когда кнопка, LinkButton или ImageButton которого `CommandName` свойство имеет значение выбирается из внутри повторителя или DataList, S повторителя или DataList `ItemCommand` события. Для DataList Если `CommandName` имеет значение определенного значения, а также могут возникать дополнительные события. Специальные `CommandName` значений свойств: в частности:

- Отмена вызывает `CancelCommand` событий
- Изменить вызывает `EditCommand` событий
- Обновить вызывает `UpdateCommand` событий

Следует помнить, что эти события вызываются *в дополнение к* `ItemCommand` событий.

Добавление `EditItemTemplate` две кнопки веб-элементов управления, из которого `CommandName` обновления и другие устройства, задайте значение "Отмена". После добавления этих двух элементов управления Web кнопку Конструктор должен выглядеть примерно следующее:


[![Добавление, обновление и Отмена кнопок EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Рис. 11**: Добавление обновления и кнопки "Отмена" для `EditItemTemplate` ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


С `EditItemTemplate` завершения вашей DataList s должна выглядеть следующим образом:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Шаг 5: Добавление устройства в режим редактирования

На этом этапе нашей DataList имеет определенное через интерфейс редактирования ее `EditItemTemplate`, однако здесь s в настоящее время невозможно для пользователя, посетите нашу страницу для указания того, он хочет изменить сведения о продукте s. Нам нужно добавить кнопку "Изменить" для каждого продукта, что, при щелчке модули подготовки, DataList элементов в режиме редактирования. Начните с добавления кнопку "Изменить", чтобы `ItemTemplate`, с помощью конструктора или декларативно. Не забудьте задать s кнопка "Изменить" `CommandName` свойств для редактирования.

После добавления этой кнопки редактирования занять некоторое время для просмотра страницы через браузер. В результате этого добавления каждого продукта списке должны содержаться кнопку "Изменить".


[![Добавление, обновление и Отмена кнопок EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Рис. 12**: Добавление обновления и кнопки "Отмена" для `EditItemTemplate` ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Нажав кнопку вызывает обратную передачу, но *не* перевести в режим редактирования списка продукции. Чтобы сделать доступным для редактирования продукт, необходимо:

1. Задать DataList s [ `EditItemIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) индексу `DataListItem` выполнен щелчок, кнопка "Изменить".
2. Привяжите данные к элементу управления DataList. Когда в DataList повторно подготовленных к просмотру `DataListItem` которого `ItemIndex` соответствует DataList s `EditItemIndex` будет отображаться с помощью его `EditItemTemplate`.

Так как элемент управления DataList s `EditCommand` событие вызывается, когда нажата кнопка "Изменить", создайте `EditCommand` обработчик событий следующим кодом:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

`EditCommand` Обработчик событий передается в объект типа `DataListCommandEventArgs` вторым параметром ввода, которая содержит ссылку на `DataListItem` была нажата, кнопка "Изменить" (`e.Item`). Обработчик события сначала задает DataList s `EditItemIndex` для `ItemIndex` из редактируемого `DataListItem` и затем выполняет повторную привязку данных к элементу управления DataList путем вызова DataList s `DataBind()` метод.

После добавления этого обработчика событий, вернитесь на страницу в браузере. Щелкнув ссылку "Изменить" позволяет выборе продукта для редактирования (см. рис. 13).


[![Щелкнув делает кнопку Правка продукта для редактирования](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Рис. 13**: нажмите кнопку «Изменить» делает редактируемый продукта ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Шаг 6: Сохранение изменений s пользователя

На этом этапе; нажатию кнопки "Отмена" или s продукта обновления не выполняет никаких действий Чтобы добавить эту функцию, необходимо ли создавать обработчики событий для DataList s `UpdateCommand` и `CancelCommand` события. Начните с создания `CancelCommand` обработчик событий, который будет выполняться при нажатии кнопки "Отмена" s продукта и его задачу с возвратом DataList состояние до редактирования.

Чтобы отобразить все элементы в режиме только для чтения DataList, необходимо:

1. Задать DataList s [ `EditItemIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) в индекс несуществующей `DataListItem` индекса. `-1` является безопасным, поскольку `DataListItem` индексы начинаются с `0`.
2. Привяжите данные к элементу управления DataList. Так как нет `DataListItem` `ItemIndex` es соответствуют DataList s `EditItemIndex`, весь DataList будут отображены в режиме только для чтения.

Эти действия могут быть выполнены с помощью следующего кода обработчика событий:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

В результате этого добавления щелкнув возвращает кнопку "Отмена" DataList в состояние до редактирования.

Последний обработчик событий, необходимо завершить — `UpdateCommand` обработчика событий. Этот обработчик событий должен:

1. Программный доступ к имени продукта, введенные пользователем и цена, а также продукта s `ProductID`.
2. Запуск процесса обновления путем вызова соответствующего `UpdateProduct` перегрузки в `ProductsBLL` класса.
3. Задать DataList s [ `EditItemIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) в индекс несуществующей `DataListItem` индекса. `-1` является безопасным, поскольку `DataListItem` индексы начинаются с `0`.
4. Привяжите данные к элементу управления DataList. Так как нет `DataListItem` `ItemIndex` es соответствуют DataList s `EditItemIndex`, весь DataList будут отображены в режиме только для чтения.

Шаги 1 и 2 несут ответственность за сохранение пользователь s изменений; После изменения были сохранены и идентичны шагов, выполненных в шагах 3 и 4 возвращать DataList состояние до редактирования `CancelCommand` обработчика событий.

Чтобы получить имя обновленного продукта и цену, необходимо использовать `FindControl` метод программно ссылка, элементы управления в Интернете в текстовом поле в `EditItemTemplate`. Нам также нужно получить продукта s `ProductID` значение. Когда мы изначально привязан элемент управления ObjectDataSource DataList, Visual Studio назначенный DataList s `DataKeyField` свойства на значение первичного ключа из источника данных (`ProductID`). Это значение может быть извлечен из DataList s `DataKeys` коллекции. Теперь пора убедитесь, что `DataKeyField` на самом деле свойству `ProductID`.

Следующий код реализует четыре шага:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Обработчик события запускается посредством считывания продукта s `ProductID` из `DataKeys` коллекции. Далее, два текстовых поля в `EditItemTemplate` указываются и их `Text` свойства, которые хранятся в локальных переменных `productNameValue` и `unitPriceValue`. Мы используем `Decimal.Parse()` метод для чтения значения из `UnitPrice` текстовое поле, поэтому, если значение, введенное указан символ валюты, его можно по-прежнему правильно преобразовываться в `Decimal` значение.

> [!NOTE]
> Значения из `ProductName` и `UnitPrice` текстовые поля только если будут использоваться для переменных productNameValue и unitPriceValue свойства текстового поля указано значение. В противном случае — значение `Nothing` используется для переменных, который действует как обновление данных с базой данных `NULL` значение. То есть наш код обрабатывает преобразует пустые строки в базу данных `NULL` значения, которые по умолчанию редактирования интерфейса в элементах управления GridView, DetailsView и FormView.


После считывания значений `ProductsBLL` класса s `UpdateProduct` вызывается метод, передав имя продукта s, цена и `ProductID`. Обработчик события завершения, возвращая DataList состояние до редактирования с помощью точное средств, как и в `CancelCommand` обработчика событий.

С `EditCommand`, `CancelCommand`, и `UpdateCommand` завершить обработчики событий, посетитель можно изменить имя и цену продукта. Данные 14-16 Показать редактирования рабочего процесса в действии.


[![При первом посещении страницы, все продукты находятся в режиме только для чтения](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Рис. 14**: при первом просмотре страницы, все продукты находятся в режиме только для чтения ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Чтобы обновить имя или цену продукта, нажмите кнопку редактирования](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Рис. 15**: чтобы обновить имя продукта или цену, нажмите кнопку Изменить ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![После изменения значения, щелкните обновление, чтобы вернуться в режим только для чтения](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**На рисунке 16**: после изменения значения, нажмите кнопку обновления, чтобы вернуться в режим только для чтения ([Просмотр полноразмерное изображение](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Шаг 7: Добавление возможности удаления

Инструкции по добавлению возможности удаления DataList аналогичны для добавления возможностей редактирования. Иными словами, необходимо добавить кнопку «Удалить» для `ItemTemplate` , при щелчке:

1. Считывает данные из соответствующего продукта s `ProductID` через `DataKeys` коллекции.
2. Выполняет удаление с помощью вызова `ProductsBLL` класса s `DeleteProduct` метод.
3. Выполняет повторную привязку данных к элементу управления DataList.

S начните с добавления кнопку «Удалить», чтобы позволить `ItemTemplate`.

При нажатии кнопки которого `CommandName` -Edit, обновление, или "Отмена" вызывает DataList s `ItemCommand` событий, а также дополнительное событие (например, в том случае, когда с помощью редактирования `EditCommand` событие также). Аналогичным образом все кнопки, LinkButton или ImageButton в DataList которого `CommandName` свойству удалить причины `DeleteCommand` срабатывание события (вместе с `ItemCommand`).

Добавьте кнопку «Удалить» рядом с «изменить» в `ItemTemplate`, параметр его `CommandName` свойство для удаления. После добавления этой кнопки управления DataList s `ItemTemplate` декларативный синтаксис должен выглядеть так:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Создайте обработчик событий для DataList s `DeleteCommand` событие, используя следующий код:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Нажав кнопку «Удалить» вызывает обратную передачу и срабатывает в DataList s `DeleteCommand` событий. В обработчике событий, пользователь щелкнул продукт s `ProductID` значение осуществляется из `DataKeys` коллекции. После этого продукта удаляется вызовом `ProductsBLL` класса s `DeleteProduct` метод.

После удаления продукта, его важно, что мы привяжите данные к элементу управления DataList s (`DataList1.DataBind()`), в противном случае элемент управления DataList будут отображать продукта, которая была удалена.

## <a name="summary"></a>Сводка

DataList не имеет точки, и нажмите кнопку редактирования и удаления поддержки, которыми пользуются GridView, с помощью коротких большой объем кода его можно улучшить эти возможности. В этом учебнике мы узнали, как для создания двух столбцов списка продуктов, может быть удален, и может редактироваться, название и цену. Добавление, изменение и удаление поддержки заключается в том числе соответствующий веб-элементов управления в `ItemTemplate` и `EditItemTemplate`, создание соответствующих обработчиков событий, чтение значения пользователь ввел и первичных ключей и взаимодействие с бизнес Логики.

Хотя мы добавили основные изменения и удаления возможности DataList, он не имеет дополнительных функций. Например, отсутствует поле ввода проверка -, если пользователь введет ценой слишком много ресурсов, будет создано исключение `Decimal.Parse` при попытке преобразования слишком дорого в `Decimal`. Аналогичным образом Если имеется проблема при обновлении данных на бизнес-логики или уровней доступа к данным пользователя откроется экран Стандартная ошибка. Без каких-либо подтверждения на кнопке удаления случайное удаление продукта слишком наверняка.

В будущем мы рассмотрим, как улучшить редактирования пользователем учебники снизиться.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Зак Джонс, Алексей Pespisa и Рэнди Шмидт. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](performing-batch-updates-cs.md)
