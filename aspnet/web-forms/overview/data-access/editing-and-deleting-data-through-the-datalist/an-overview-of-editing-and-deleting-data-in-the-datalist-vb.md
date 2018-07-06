---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Общие сведения о об изменении и удалении данных в DataList (VB) | Документация Майкрософт
author: rick-anderson
description: Хотя DataList отсутствует встроенный редактирование и удаление возможностей, в этом руководстве мы будет показано, как создать элемент управления DataList, которая поддерживает редактирование и удаление o...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: aa1e07be5b634a0e66f7c6f03f80a1aa2c741189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804502"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Общие сведения о об изменении и удалении данных в DataList (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) или [скачать PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Хотя DataList отсутствует встроенный редактирование и удаление возможностей, в этом руководстве будет показано, как создать элемент управления DataList, поддерживает, правку и удаление базовых данных.


## <a name="introduction"></a>Вступление

В [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) учебнике мы рассмотрели способы вставки, обновления и удаления данных с помощью архитектуры приложения, нового ObjectDataSource и GridView, DetailsView и FormView элементы управления. Элемент управления ObjectDataSource и эти три данных веб-элементы управления реализация интерфейсов изменения простых данных был очень простой и участвует просто отсчет флажок смарт-теге. Без кода, необходимого для записи.

К сожалению DataList не имеет встроенной редактирования и удаления возможности, обеспечиваемые в элементе управления GridView. Эта функция отсутствует из-за частично тот факт, что DataList relic из предыдущей версии платформы ASP.NET, когда декларативный источников данных и данных без изменения страницы были недоступны. Хотя элемент управления DataList в ASP.NET 2.0 не обеспечивают без дополнительной настройки возможностей изменения данных элемент GridView, можно использовать методики ASP.NET 1.x для включения такой функции. Этот подход требует немного кода, но, как мы увидим в этом руководстве, DataList предусмотрена некоторые события и свойства для помощи в этом процессе.

В этом руководстве будет показано, как создать элемент управления DataList, поддерживает, правку и удаление базовых данных. Последующих учебных курсах будут рассмотрены более сложные редактирование и удаление сценариев, включая проверки поля ввода, аккуратная обработка исключений, порождаемых доступа к данным или уровни бизнес-логики и т. д.

> [!NOTE]
> Как элемент управления DataList элемент управления Repeater не имеет готовой поле функциональные возможности для вставки, обновления или удаления. Хотя можно добавить такие функции, в элементе DataList имеются свойства и события, не найден в элементу управления Repeater, которые упрощают добавление таких возможностей. Таким образом в этом учебнике и последующие, которые отслеживают изменения и удаления основное внимание уделяется строго DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Шаг 1: Создание веб-страниц редактирования и удаления учебники

Прежде чем приступать, показывающих, как обновление и удаление данных с элементом управления DataList, позвольте s уделим несколько минут созданию в наш проект веб-сайта, что нам понадобится для этого учебника и нескольких следующих страниц ASP.NET. Начните с добавления новой папки с именем `EditDeleteDataList`. Добавьте следующие страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Добавление страниц ASP.NET для использования учебников](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Рис. 1**: Добавление страниц ASP.NET для использования учебников


Как и в других папках, `Default.aspx` в `EditDeleteDataList` папки перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Поэтому добавьте данный пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s режиме конструктора.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Рис. 2**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Наконец, добавьте страницы как записи, чтобы `Web.sitemap` файл. В частности, добавьте следующую разметку после отчеты «основной/подробности» с помощью элементов управления DataList и Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элементы для элементов управления DataList, редактирования и удаления учебных курсов.


![Карта узла теперь включают записи для элементов управления DataList, редактирования и удаления учебных курсов](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Рис. 3**: карта узла теперь включают записи для элементов управления DataList, редактирования и удаления учебных курсов


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Шаг 2: Проверка методов для обновления и удаления данных

Редактирование и удаление данных с помощью GridView так просто, поскольку внутри себя, GridView и ObjectDataSource работать совместно. Как уже говорилось в [Проверка события, связанные с вставки, обновления и удаления](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) руководстве при нажатии кнопки обновления строк GridView, автоматически назначает его поля, которые использованы двухсторонней привязкой данных к `UpdateParameters` коллекцию управления ObjectDataSource, а затем вызывает ObjectDataSource s `Update()` метод.

К сожалению DataList не предоставляет этих встроенных функций. Наша ответственность, чтобы убедиться, что пользователь s значения присваиваются параметры s ObjectDataSource и его `Update()` вызывается метод. Чтобы помочь нам в эту задачу, DataList обеспечивает перечисленные ниже свойства и события.

- **[ `DataKeyField` Свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  при обновлении или удалении, мы должны быть способны уникально идентифицировать каждого элемента в элементе управления DataList. Значение этого свойства поле первичного ключа отображаемых данных. Таким образом будет заполнять DataList s [ `DataKeys` коллекции](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) с указанным `DataKeyField` значение для каждого элемента управления DataList.
- **[ `EditCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  срабатывает, если кнопки, LinkButton или ImageButton которого `CommandName` свойству нажатии редактирования.
- **[ `CancelCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  срабатывает, если кнопки, LinkButton или ImageButton которого `CommandName` свойству щелкнул "Отмена".
- **[ `UpdateCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  срабатывает, если кнопки, LinkButton или ImageButton которого `CommandName` свойству щелчке обновления.
- **[ `DeleteCommand` Событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  срабатывает, если кнопки, LinkButton или ImageButton которого `CommandName` свойству нажатии Delete.

С помощью этих свойств и событий, существует четыре подхода, которые можно использовать для обновления и удаления данных из элемента управления DataList.

1. **С помощью методов ASP.NET 1.x** DataList существовали до ASP.NET 2.0 и элементов управления ObjectDataSource и смог обновление и удаление данных полностью основана на программных средств. Этот метод вообще ditches ObjectDataSource и требуется, мы привязать данные к элементу управления DataList непосредственно из уровня бизнес-логики, как при получении данных для отображения и при обновлении или удалении записи.
2. **С помощью одного элемента управления ObjectDataSource на странице Выбор, обновление и удаление** хотя DataList нет встроенного редактирования GridView s и удаление возможностей, здесь s без причины, мы можем t добавить их в сами. При таком подходе мы так же, как использовать элемент управления ObjectDataSource в примерах GridView, но необходимо создать обработчик событий для элементов управления DataList s `UpdateCommand` событий, где мы задайте параметры s ObjectDataSource и вызовите его `Update()` метод.
3. **С помощью элемента управления ObjectDataSource для выбор, но обновление и удаление непосредственно к BLL** при использовании параметра 2, нам нужно написать небольшой фрагмент кода в `UpdateCommand` событий, присвоение значения параметров и т. д. Вместо этого мы можно использовать элемент управления ObjectDataSource для выбора, но вызовов обновления и удаления непосредственно к BLL (например, с параметром 1). По моему мнению, обновление данных, работая с BLL приводит к более удобочитаемый код, чем назначение ObjectDataSource s `UpdateParameters` и вызов его `Update()` метод.
4. **С помощью декларативным образом через несколько элементов управления ObjectDataSource** все предыдущие три подхода требуют немного кода. D вместо сохранять использовании в качестве гораздо декларативный синтаксис максимально, последнее значение — для включения нескольких элементов управления ObjectDataSource на странице. Первый элемент управления ObjectDataSource извлекает данные из BLL и привязывает его к DataList. Для обновления, является добавить другой элемент управления ObjectDataSource, но добавить непосредственно в элементе управления DataList s `EditItemTemplate`. Чтобы включить удаление поддержки, пока другой элемент управления ObjectDataSource, которое необходимо в `ItemTemplate`. При таком подходе эти внедренные использование ObjectDataSource `ControlParameters` для декларативной привязки параметров s ObjectDataSource для элементов управления пользовательского ввода (вместо того, чтобы задать их программным образом в элементе управления DataList s `UpdateCommand` обработчик событий). Этот подход по-прежнему требует немного кода, необходимо вызвать внедренный элемент управления ObjectDataSource s `Update()` или `Delete()` команды, но требует гораздо меньше, чем с другими три подходы. Недостатком здесь является то, что несколько элементов управления ObjectDataSource загромождать странице отвлекать от общую читаемость.

Если принудительно использовать только один из этих подходов, я d выберите параметр 1, поскольку он обеспечивает наибольшую гибкость и DataList первоначально предназначалась для размещения этого шаблона. Хотя элемент управления DataList была расширена для работы с элементами управления источника данных ASP.NET 2.0, он не имеет всех точек расширения или функций официальный веб-элементов управления (GridView, DetailsView и FormView) данных ASP.NET 2.0. Параметры 2 – 4, не позволил компании merit, однако.

Это и будущих правки и удаления учебных курсов будет использовать элемент управления ObjectDataSource для получения данных для отображения и прямых вызовов к BLL для обновления и удаления данных (вариант 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Шаг 3: Добавление элемента управления DataList и настройка управления ObjectDataSource

В этом руководстве мы создадим элемент управления DataList, перечислены сведения о продукте и для каждого продукта, предоставляет пользователю возможность изменить имя и цену и полностью удалить продукт. В частности мы извлечения записей для отображения с помощью нового ObjectDataSource, но выполнить обновление и удалить действия, работая с BLL. Прежде чем думать о реализации возможности редактирования и удаления для элемента управления DataList, позвольте s сначала перейти на страницу для отображения продуктов в интерфейсе только для чтения. Так как мы ve проверяются следующие действия в предыдущих учебных курсах, я покажу через них быстро.

Сначала откройте `Basics.aspx` странице в `EditDeleteDataList` папку и в режиме конструктора добавьте на страницу элемент управления DataList. S смарт-теге элемента управления DataList, создайте новый ObjectDataSource. Так как мы работаем с данными о продуктах, настройте его для использования `ProductsBLL` класса. Для получения *все* выберите продукты, `GetProducts()` метод на вкладке "SELECT".


[![Настройка ObjectDataSource на использование класса ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Рис. 4**: Настройка ObjectDataSource для использования `ProductsBLL` класс ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Возвращает сведения о продукте, с помощью метода GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Рис. 5**: возвращают сведения о продукте с помощью `GetProducts()` метод ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


Элемент управления DataList, например GridView, не предназначена для вставки новых данных; Таким образом, выберите параметр из раскрывающегося списка на вкладке "Вставка" (None). Также выберите (нет) для тех вкладок, UPDATE и DELETE так, как обновления и удаления будут выполняться программным способом через слой бизнес-ЛОГИКИ.


[![Убедитесь, что раскрывающемся списке указаны в элемент управления ObjectDataSource s INSERT, UPDATE и удаление вкладок заданы (нет)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Рис. 6**: Убедитесь, что список раскрывающегося списка в элемент управления ObjectDataSource s INSERT, UPDATE и удаление вкладок заданы (нет) ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


После настройки ObjectDataSource, нажмите кнопку Готово, возвращение в конструктор. Как мы видели в предыдущих примерах, после завершения настройки ObjectDataSource, Visual Studio автоматически ve создает `ItemTemplate` для элемента управления DropDownList, отображение всех полей данных. Введите здесь `ItemTemplate` на тот, который отображает только s имя и цену продукта. Кроме того, задайте `RepeatColumns` значение 2.

> [!NOTE]
> Как уже говорилось в *Обзор Вставка, обновление и удаление данных* руководства, при изменении данных с помощью нашей архитектуры требует, что мы удалим ObjectDataSource `OldValuesParameterFormatString` свойства из ObjectDataSource s декларативная разметка (или выполнить его сброс до значения по умолчанию и `{0}`). В этом руководстве тем не менее, мы используем только для получения данных элемент управления ObjectDataSource. Таким образом, нам не нужно изменять ObjectDataSource s `OldValuesParameterFormatString` значение свойства (хотя он повредить для этого).


После замены по умолчанию элемент управления DataList `ItemTemplate` на настроенный, на странице должна выглядеть следующим образом:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Отвлекитесь и просмотрите ход работы через браузер. Как показано на рис. 7, DataList отображает продукта имя и цену за единицу для каждого продукта в двух столбцах.


[![Названия продуктов и цены отображаются два столбца элемента управления DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Рис. 7**: названия продуктов и цены отображаются два столбца элемента управления DataList ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> Элемент управления DataList имеет ряд свойств, которые требуются в процессе обновления и удаления, и эти значения сохраняются в состоянии представления. Таким образом Если сборку элемент управления DataList, которое поддерживает изменение или удаление данных, очень важно включенное состояние представления элемента управления DataList s.  
>   
>  Внимательный читатель вспомнить, что нам удалось отключить состояние представления, при создании редактируемых элементов управления GridView, DetailsViews и FormView среды. Это обусловлено тем, может включать элементы управления ASP.NET 2.0 Web *состояние элемента управления*, который сохраняется состояние во время обратной передачи, такие как состояние представления, но считается essential.


Отключение просмотра состояния в GridView просто пропускает сведения о состоянии тривиальный, но сохраняет состояние элемента управления (который включает в себя состояние, необходимое для редактирования и удаления). DataList, созданный в годах ASP.NET 1.x, не использует состояние элемента управления и поэтому должна иметь состояние просмотра включено. См. в разделе [vs состояние элемента управления. Состояние представления](https://msdn.microsoft.com/library/1whwt1k7.aspx) Дополнительные сведения о назначении состояние элемента управления и как она отличается от состояния представления.

## <a name="step-4-adding-an-editing-user-interface"></a>Шаг 4: Добавление пользовательский интерфейс редактирования

Элемент управления GridView представляет собой коллекцию полей (полей BoundField, CheckBoxFields, поля TemplateField и т. д.). Эти поля можно настроить их разметку в зависимости от их режима. Например в режиме только для чтения поле BoundField отображает его значения поля данных как текст; в режиме редактирования, он отображает TextBox Web со свойством `Text` присваивается значение поля данных.

Элемент управления DataList, с другой стороны, отображает его элементы, с помощью шаблонов. Только для чтения элементы отображаются с использованием `ItemTemplate` в то время как элементы в режиме редактирования отображаются с помощью `EditItemTemplate`. На этом этапе нашей DataList имеет только `ItemTemplate`. Для поддержки функциональных возможностей редактирования уровня элемента, нам нужно добавить `EditItemTemplate` , содержащая разметку, отображаемую для редактирования элемента. В этом учебнике мы будем использовать текстовое поле веб-элементы управления для редактирования продукта s имя и цену за единицу.

`EditItemTemplate` Могут создаваться декларативно или с помощью конструктора (путем выбора параметра Правка шаблонов в смарт-теге элемента управления DataList s). Чтобы использовать параметр Правка шаблонов, сначала щелкните ссылку Изменить шаблоны в смарт-тег, а затем выберите `EditItemTemplate` элемент из раскрывающегося списка.


[![Необязательно для работы с DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Рис. 8**: необязательно к работе с DataList s `EditItemTemplate` ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


Затем введите название продукта: и Price: и перетащите два элемента управления TextBox из области элементов в `EditItemTemplate` интерфейс в конструкторе. Набор текстовых полей `ID` свойства `ProductName` и `UnitPrice`.


[![Добавление текстового поля для s имя и цену продукта](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Рис. 9**: Добавление текстового поля для продукта, имя и цену ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


Нам нужно привязать соответствующие значения поля данных продукта для `Text` свойства из двух текстовых полей. Текстовые поля смарт-тегами, щелкните ссылку Edit DataBindings, а затем свяжите поле соответствующие данные `Text` свойства, как показано на рис. 10.

> [!NOTE]
> При привязке `UnitPrice` поле данных со стоимостью TextBox s `Text` поле, вы может отформатировать его в виде значения валюты (`{0:C}`), число с общим (`{0:N}`), или оставить его неформатированного.


![Привязки ProductName и UnitPrice данных полей к свойствам текста из текстовых полей](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Рис. 10**: привязка `ProductName` и `UnitPrice` поля данных для `Text` свойства из текстовых полей


Обратите внимание на то, каким образом диалоговое окно Edit DataBindings на рис. 10 *не* установить флажок двусторонней привязки данных, которая присутствует, при редактировании TemplateField в GridView или DetailsView или шаблона в FormView для включения. Функция двусторонней привязки данных допустимое значение, введенное в элемент управления Web автоматически назначен соответствующий элемент управления ObjectDataSource s `InsertParameters` или `UpdateParameters` при вставке или обновлении данных. DataList не поддерживает двусторонней привязки данных, как мы увидим позже в этом руководстве после пользователь вносит ей изменения и готов для обновления данных, нам потребуется для программного доступа к эти текстовые поля `Text` свойства и передайте их значения для соответствующие `UpdateProduct` метод в `ProductsBLL` класса.

Наконец, нам нужно добавить обновления и «Отмена», чтобы `EditItemTemplate`. Как мы видели в [Master/Detail, с использованием маркированного списка основных записей с элементом управления DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) руководстве описано, когда кнопка, LinkButton или ImageButton которого `CommandName` свойству внутри из элемента управления Repeater и DataList, Элемент управления Repeater и DataList s `ItemCommand` события. Для элементов управления DataList Если `CommandName` свойству присваивается определенное значение, также могут возникать дополнительные события. Специальные `CommandName` включают значения свойств, помимо прочего:

- Отмена вызывает `CancelCommand` событий
- Изменение вызывает `EditCommand` событий
- Обновить вызывает `UpdateCommand` событий

Имейте в виду, что эти события вызываются *в дополнение к* `ItemCommand` событий.

Добавить `EditItemTemplate` две кнопки веб-элементов управления, из которого `CommandName` обновления и другие устройства, значение "Отмена". После добавления этих двух элементов управления Web кнопку Конструктор должен выглядеть следующим образом:


[![Добавить обновление и «Отмена», чтобы EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Рис. 11**: добавить обновление и Отмена кнопок `EditItemTemplate` ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


С помощью `EditItemTemplate` полный вашего элемента управления DataList s должна выглядеть следующим образом:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Шаг 5: Добавление базовую инфраструктуру в режим редактирования

На этом этапе нашей DataList содержит интерфейс редактирования, определенный с помощью его `EditItemTemplate`, однако здесь s в настоящее время невозможно для пользователя, посетите нашу страницу для указания, что он хочет изменить сведения о продукте s. Нам нужно добавить кнопку "Изменить" для каждого продукта, что, при нажатии отображает этот элемент управления DataList элементов в режиме редактирования. Начните с добавления кнопка «Изменить» к `ItemTemplate`, с помощью конструктора или декларативно. Не забудьте задать "Изменить" s `CommandName` свойств для редактирования.

После добавления этого кнопка "Изменить", Отвлекитесь и просмотрите страницу через обозреватель. В результате этого добавления каждым списком продуктов должен включать кнопка «изменить».


[![Добавить обновление и «Отмена», чтобы EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Рис. 12**: добавить обновление и Отмена кнопок `EditItemTemplate` ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Нажав кнопку вызывает обратную передачу, но *не* перевести в режим редактирования списка продукции. Чтобы разрешить изменение продукта, нам нужно:

1. Набор элементов управления DataList s [ `EditItemIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) на индекс `DataListItem` просто нажать кнопку, кнопку "Изменить".
2. Привяжите данные к DataList. Когда элемент управления DataList повторно подготовленных к просмотру, `DataListItem` которого `ItemIndex` соответствует DataList s `EditItemIndex` отрисовывается с помощью его `EditItemTemplate`.

С момента DataList s `EditCommand` событие возникает при нажатии кнопки Edit, создайте `EditCommand` обработчик событий следующим кодом:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand` Обработчику события передаются в объект типа `DataListCommandEventArgs` как второй входной параметр, который включает в себя ссылку на `DataListItem` была нажата, кнопка "Изменить" (`e.Item`). Обработчик событий сначала задает DataList s `EditItemIndex` для `ItemIndex` из редактируемый `DataListItem` и затем выполняет повторную привязку данных к элементу управления DataList, вызвав DataList s `DataBind()` метод.

После добавления этого обработчика событий, вернитесь на страницу в браузере. Щелкнув ссылку "Изменить" делает щелчке продукта редактируемой (см. рис. 13).


[![Щелкнув делает кнопка редактирования редактируемой продукта](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Рис. 13**: Выбор "Изменить" делает редактируемый продукта ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Шаг 6: Сохранение изменений s пользователя

Щелкнув s продукта обновления или кнопки "Отмена" не выполняет никаких действий на этом этапе; Чтобы добавить эту функцию, необходимо создать обработчики событий для элементов управления DataList s `UpdateCommand` и `CancelCommand` события. Начните с создания `CancelCommand` обработчик событий, который будет выполняться при нажатии кнопки "Отмена" s продукта и его необходимо с возвратом DataList состояние до редактирования.

Чтобы отобразить все элементы в режиме только для чтения элемент управления DataList, нам нужно:

1. Набор элементов управления DataList s [ `EditItemIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) на индекс на несуществующую `DataListItem` индекса. `-1` является безопасным вариантом, поскольку `DataListItem` индексы начинаются с `0`.
2. Привяжите данные к DataList. Так как нет `DataListItem` `ItemIndex` es соответствуют DataList s `EditItemIndex`, весь элемент управления DataList отображается в режиме только для чтения.

Эти действия можно использовать следующий код обработчика событий:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

В результате этого добавления щелкнув возвращает кнопку "Отмена" DataList в состояние до редактирования.

Последний обработчик событий, необходимо выполнить `UpdateCommand` обработчик событий. Этот обработчик событий должен:

1. Программный доступ к имени продукта, введенный пользователем и цена, а также продукта s `ProductID`.
2. Начать процесс обновления путем вызова соответствующего `UpdateProduct` перегрузки в `ProductsBLL` класса.
3. Набор элементов управления DataList s [ `EditItemIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) на индекс на несуществующую `DataListItem` индекса. `-1` является безопасным вариантом, поскольку `DataListItem` индексы начинаются с `0`.
4. Привяжите данные к DataList. Так как нет `DataListItem` `ItemIndex` es соответствуют DataList s `EditItemIndex`, весь элемент управления DataList отображается в режиме только для чтения.

Шаги 1 и 2 несут ответственность за сохранение пользователем изменений s; После изменения были сохранены и идентичны шагов, выполненных в шагах 3 и 4 возвращать DataList состояние до редактирования `CancelCommand` обработчик событий.

Чтобы получить обновленные имя и цену продукта, нам нужно использовать `FindControl` метод программно ссылки, элементы управления TextBox Web в `EditItemTemplate`. Нам также нужно получить продукта s `ProductID` значение. Если мы изначально привязан элемент управления ObjectDataSource DataList, Visual Studio назначается DataList s `DataKeyField` присваивается значение первичного ключа из источника данных (`ProductID`). Это значение может быть извлечен из элемента управления DataList s `DataKeys` коллекции. Отвлекитесь и убедитесь, что `DataKeyField` действительно свойству `ProductID`.

Следующий код реализует четыре шага:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Обработчик событий начинает с чтения в продукта s `ProductID` из `DataKeys` коллекции. Далее, два текстовых поля в `EditItemTemplate` указываются и их `Text` свойства, которые хранятся в локальных переменных, `productNameValue` и `unitPriceValue`. Мы используем `Decimal.Parse()` метод для чтения значения из `UnitPrice` текстовое поле, поэтому, если значение, введенное находится символ валюты, его можно по-прежнему правильно преобразовываться в `Decimal` значение.

> [!NOTE]
> Значения из `ProductName` и `UnitPrice` текстовые поля только назначаются переменным productNameValue и unitPriceValue, если свойства текстового поля задано значение. В противном случае — значение `Nothing` используется для переменных, который действует как обновление данных с базой данных `NULL` значение. То есть наш код обрабатывает преобразует пустые строки в базу данных `NULL` значения, которые по умолчанию редактирования интерфейса в элементах управления GridView, DetailsView и FormView.


После считывания значений, `ProductsBLL` класс s `UpdateProduct` вызывается метод, передав имя продукта s, цены, и `ProductID`. Обработчик событий завершения путем возврата в состояние до редактирования с помощью средств, которые точно как и в DataList `CancelCommand` обработчик событий.

С помощью `EditCommand`, `CancelCommand`, и `UpdateCommand` завершения обработчики событий, посетитель можно изменить имя и цену продукта. Рис. 14-16 Показать этот редактирования рабочий процесс в действии.


[![При первом просмотре страницы, все продукты в режиме только для чтения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Рис. 14**: при первом просмотре страницы, все продукты, в режиме только для чтения ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Чтобы обновить продукт s имени или по цене, нажмите кнопку "Изменить"](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Рис. 15**: чтобы обновить имя продукта или цены, нажмите кнопку Изменить ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![После изменения значения, нажмите кнопку обновления, чтобы вернуться в режим только для чтения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Рис. 16**: после изменения значения, нажмите кнопку обновления, чтобы вернуться в режим только для чтения ([Просмотр полноразмерного изображения](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Шаг 7: Добавление возможности удаления

Шаги для добавления возможностей удалить элемент управления DataList, аналогичны для добавления возможности редактирования. Короче говоря, нам нужно добавить кнопку «Удалить» для `ItemTemplate` , при щелчке:

1. Считывает в соответствующем продукте s `ProductID` через `DataKeys` коллекции.
2. Выполняет удаление путем вызова `ProductsBLL` класс s `DeleteProduct` метод.
3. Выполняет повторную привязку данных к элементу управления DataList.

S начните с добавления кнопку Delete, чтобы позволить `ItemTemplate`.

При нажатии кнопки, `CommandName` -Edit, обновление, или "Отмена" вызывает DataList s `ItemCommand` событий, а также дополнительное событие (например, в том случае, при использовании редактирования `EditCommand` события также). Аналогичным образом все кнопки, LinkButton или ImageButton в элементе управления DataList, `CommandName` свойству удалить причины `DeleteCommand` событие (вместе с `ItemCommand`).

Добавьте кнопку «Удалить» рядом с "Изменить" в `ItemTemplate`, устанавливая его `CommandName` свойства для инструкции Delete. После добавления этой кнопки управления DataList s `ItemTemplate` декларативный синтаксис должен выглядеть так:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Создайте обработчик событий для элементов управления DataList s `DeleteCommand` событие, используя следующий код:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Нажмите кнопку Delete вызывает обратную передачу и запускает DataList s `DeleteCommand` событий. В обработчике событий щелчка продукта s `ProductID` значение осуществляется из `DataKeys` коллекции. После этого продукта удаляется путем вызова `ProductsBLL` класс s `DeleteProduct` метод.

После удаления продукта, его s, важно, что мы привяжите данные к элементу управления DataList (`DataList1.DataBind()`), в противном случае элемент управления DataList продолжат отображаться продукта, которая была удалена.

## <a name="summary"></a>Сводка

DataList отсутствует точка и нажмите кнопку редактирования и удаления поддержки пользуются GridView, с небольшой короткий код его можно улучшить, добавив эти функции. В этом учебнике мы рассмотрели создание двух столбцов списка продуктов, может быть удален, и может редактироваться, название и цену. Добавление, изменение и удаление поддержки сводится к, включая соответствующие веб-элементов управления в `ItemTemplate` и `EditItemTemplate`, создание соответствующих обработчиков событий, чтение значений введенный пользователем и первичного ключа и взаимодействии с бизнес Логики.

Мы добавили основные изменения и удаления возможности для элемента управления DataList, однако у него нет дополнительных функций. Например, не проверяется поле ввода — Если пользователь вводит цену слишком дорогим, будет создано исключение `Decimal.Parse` при попытке преобразования слишком дорого в `Decimal`. Аналогичным образом Если возникла проблема при обновлении данных на бизнес-логику или уровни доступа к данным пользователя откроется экран стандартной ошибки. Без каких-либо подтверждения "Удалить" случайного удаления продукта, вероятнее всего все слишком.

В будущем возникнуть учебники, узнаете, как для улучшения редактирования пользователем.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Зак Джонс, Алексей Pespisa и Рэнди Шмидт, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](customizing-the-datalist-s-editing-interface-cs.md)
> [Вперед](performing-batch-updates-vb.md)
