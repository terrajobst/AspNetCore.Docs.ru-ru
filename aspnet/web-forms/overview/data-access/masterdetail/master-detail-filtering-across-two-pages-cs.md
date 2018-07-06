---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: «Основной/подробности» фильтрации на двух страницах (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы Реализуйте этот шаблон с помощью элемента управления GridView для отображения списка поставщиков в базе данных. Каждая строка поставщика в GridView будет содержать Vie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 48102f64b1ec832774d9b41258503937dae4f4d5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373124"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>«Основной/подробности» фильтрации на двух страницах (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) или [скачать PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> В этом руководстве мы Реализуйте этот шаблон с помощью элемента управления GridView для отображения списка поставщиков в базе данных. Каждая строка поставщика в GridView будет содержать ссылку Просмотр продуктов, что, если он выбран, будет переводить пользователя на отдельную страницу со списком продуктов для выбранного поставщика.


## <a name="introduction"></a>Вступление

В предыдущих двух учебных курсах мы видели как [отображения отчетов «основной/подробности» на одной веб-странице, с помощью элементов управления DropDownList](master-detail-filtering-with-a-dropdownlist-cs.md) для [отображения «основных» записей и элемент управления GridView или DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) для отображения» Подробности». Другим распространенным шаблоном для обобщенных и подробных отчетов является основных записей на одной странице, а также сведения, отображаемые на другом. Форум по веб-сайта, например [на форумах ASP.NET](https://forums.asp.net/), — отличный пример этого шаблона на практике. Форумы ASP.NET состоят из множества форумов Приступая к работе, веб-формы, элементы управления представления данных и т. д. Каждый форум состоит из множества обсуждений, и каждый поток состоит из ряда сообщений. На домашней странице форумов ASP.NET содержится список форумов. Щелкнув на форуме перейти к `ShowForum.aspx` страницу, которая содержится список обсуждений этого форума. Аналогично, если щелкнуть в потоке, отобразится `ShowPost.aspx`, сообщениями для потока, который был щелкнут.

В этом руководстве мы Реализуйте этот шаблон с помощью элемента управления GridView для отображения списка поставщиков в базе данных. Каждая строка поставщика в GridView будет содержать ссылку Просмотр продуктов, что, если он выбран, будет переводить пользователя на отдельную страницу со списком продуктов для выбранного поставщика.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Шаг 1: Добавление`SupplierListMaster.aspx`и`ProductsForSupplierDetails.aspx`страниц к`Filtering`папки

При определении макета страниц в третьем учебнике мы добавили ряд «стартовых» страниц в `BasicReporting`, `Filtering`, и `CustomFormatting` папки. Тем не менее, мы не добавляли начальной страницы в этом руководстве в это время, поэтому Отвлекитесь и добавьте новую страницу, чтобы `Filtering` папки: `SupplierListMaster.aspx` и `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` будет список «основных» записей (поставщики) при `ProductsForSupplierDetails.aspx` будут отображаться продукты для выбранного поставщика.

При создании этих двух новых страниц не забудьте связать их с `Site.master` главной страницы.


![Добавление страниц SupplierListMaster.aspx и Productsforsupplierdetails.aspx в папку фильтрации](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Рис. 1**: добавление `SupplierListMaster.aspx` и `ProductsForSupplierDetails.aspx` страниц к `Filtering` папки


Кроме того, при добавлении новых страниц к проекту, не забудьте обновить файл карты узла, `Web.sitemap`, соответствующим образом. В этом руководстве просто добавьте `SupplierListMaster.aspx` страницы в карту узла, используя следующее содержимое XML в качестве дочернего элемента для элемента `<siteMapNode>` элемент:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Вы можете помочь автоматизировать процесс обновления файла карты узла, когда Добавление нового ASP.NET страницы с использованием [к. Скотт Аллен](http://odetocode.com/Blogs/scott/)бесплатного [макроса карты узла](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Шаг 2: Отображение списка поставщиков на странице`SupplierListMaster.aspx`

С помощью `SupplierListMaster.aspx` и `ProductsForSupplierDetails.aspx` страницы, созданные, следующим этапом является создание элемента управления GridView для поставщиков в `SupplierListMaster.aspx`. Добавьте на страницу GridView и привяжите его к элементу управления ObjectDataSource. Следует использовать данный элемент управления ObjectDataSource `SuppliersBLL` класса `GetSuppliers()` метод для возврата всех поставщиков.


[![Выбор метода Getsuppliers() класса](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Рис. 2**: выберите `SuppliersBLL` класс ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Настройка ObjectDataSource на использование метода GetSuppliers()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Рис. 3**: Настройка ObjectDataSource для использования `GetSuppliers()` метод ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Нам нужно включить ссылку под названием Просмотр продуктов в каждой строке GridView, при нажатии ведет `ProductsForSupplierDetails.aspx` передачи в выделенной строке `SupplierID` значение через строку запроса. Например, если пользователь щелкает ссылку Просмотр продуктов для поставщика Tokyo Traders (который имеет `SupplierID` равным 4), они должны отправляться `ProductsForSupplierDetails.aspx?SupplierID=4`.

Для этого добавьте [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) к GridView, что добавляет гиперссылку для каждой строки GridView. Запустить, щелкнув ссылку Правка столбцов смарт-теге элемента GridView. Затем выберите поле HyperLinkField в списке в левом верхнем углу и щелкните "Добавить", чтобы включить поле HyperLinkField в списке полей GridView.


[![Добавление поля HyperLinkField к элементу GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Рис. 4**: Добавление поля HyperLinkField к GridView ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image10.png))


Поле HyperLinkField можно настроить таким образом, чтобы использовать тот же текст или URL-адрес значения по ссылке в каждой строке GridView, или эти значения могут основываться на значениях данных, привязанных ко всем строкам. Для указания статического значения для всех строк используйте свойство `Text` или `NavigateUrl` свойства. Поскольку мы хотим текст ссылки должен быть одинаковым для всех строк, установка для свойства `Text` свойства для просмотра продуктов.


[![Значение свойства Text HyperLinkField Просмотр продуктов](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Рис. 5**: Установка для свойства `Text` свойства для просмотра продуктов ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Чтобы задать текст или значения URL-адрес должен быть основан на базовых данных, привязанных к строке GridView, укажите текст поля данных или значения URL-адрес должен быть взят из в `DataTextField` или `DataNavigateUrlFields` свойства. `DataTextField` может устанавливаться только для одного поля данных; `DataNavigateUrlFields`, тем не менее, можно присвоить разделенный запятыми список полей данных. Мы часто понадобится текст или URL-адрес на сочетании значения поля данных текущей строки и определенной статической разметки. В этом руководстве, например, мы хотим URL-адрес ссылок поля HyperLinkField быть `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, где *`supplierID`* строка каждого элемента GridView `SupplierID` значение. Обратите внимание, что нам нужно как статическим, так и управляемые данными значения здесь: `ProductsForSupplierDetails.aspx?SupplierID=` часть URL-адреса ссылки является статическим, тогда как *`supplierID`* часть является управляемой данными как его значение равно собственное значение `SupplierID` значение.

Для указания сочетания статических и управляемых данными значений, используйте `DataTextFormatString` и `DataNavigateUrlFormatString` свойства. В этих свойствах статическую разметку при необходимости введите и затем установите маркер `{0}` нужное значение поля, указанного в `DataTextField` или `DataNavigateUrlFields` свойства для отображения. Если `DataNavigateUrlFields` свойство имеет несколько полей использование `{0}` там, где требуется вставить, значение первого поля `{1}` для второго значения поля и т. д.

Применяется в этом руководстве, нам нужно установить `DataNavigateUrlFields` свойства `SupplierID`, так как это поле данных, значение которого необходимо настроить на основе строки, и `DataNavigateUrlFormatString` свойства `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Настройка HyperLinkField на включение URL-адрес соответствующие ссылки, в зависимости от SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Рис. 6**: Настройка HyperLinkField на включение правильной ссылки URL-адрес на основе при `SupplierID` ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image16.png))


После добавления HyperLinkField, вы можете настроить и изменить порядок полей GridView. В следующей разметке показан GridView после внесения некоторых незначительных изменений на уровне полей.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Отвлекитесь и просмотрите `SupplierListMaster.aspx` страницы в обозревателе. Как показано на рис. 7, страницы в настоящее время список всех поставщиков, включая ссылку Просмотр продуктов. Щелкнув Просмотр продуктов ссылке вы перейдете на `ProductsForSupplierDetails.aspx`, передав вдоль поставщика `SupplierID` в строке запроса.


[![Каждая строка поставщика содержит ссылку Просмотр продуктов](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Рис. 7**: каждая строка поставщика содержит ссылку Просмотр продуктов ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Шаг 3: Перечисление продуктов поставщика в`ProductsForSupplierDetails.aspx`

На этом этапе `SupplierListMaster.aspx` отправляет пользователей на странице `ProductsForSupplierDetails.aspx`, передав выбранного поставщика `SupplierID` в строке запроса. Последним шагом данного учебного курса является отображение продуктов в элементе управления GridView на `ProductsForSupplierDetails.aspx` которого `SupplierID` равно `SupplierID` переданного через строку запроса. Для этого необходимо добавить GridView к `ProductsForSupplierDetails.aspx` страницы, используя новый элемент управления ObjectDataSource с именем `ProductsBySupplierDataSource` , вызывающий `GetProductsBySupplierID(supplierID)` метода из `ProductsBLL` класса.


[![Добавьте новый ObjectDataSource, именуемый ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Рис. 8**: добавьте новый ObjectDataSource с именем `ProductsBySupplierDataSource` ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Выбор класса](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Рис. 9**: выберите `ProductsBLL` класс ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![У элемента управления ObjectDataSource вызвать метод GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Рис. 10**: иметь элемент управления ObjectDataSource вызывает `GetProductsBySupplierID(supplierID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Последний шаг в мастере настройки источника данных запрос на указание источника `GetProductsBySupplierID(supplierID)` метода *`supplierID`* параметра. Чтобы использовать значения строки запроса, в качестве источника параметра строки запроса и введите имя значения строки запроса для использования в текстовом поле QueryStringField (`SupplierID`).


[![Заполнение supplierID значение параметра из значения строки запроса SupplierID](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Рис. 11**: заполнения *`supplierID`* значение параметра из `SupplierID` значение строки запроса ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Это все, вот! Рис. 12 показан `ProductsForSupplierDetails.aspx` странице открываемая при щелчке ссылки Tokyo Traders из `SupplierListMaster.aspx`.


[![Отображаются продукты поставляемые Tokyo Traders](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Рис. 12**: отображаются продукты поставляемые Tokyo Traders ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Отображение информации о поставщике в`ProductsForSupplierDetails.aspx`

Как показано на рис. 12, `ProductsForSupplierDetails.aspx` просто перечислены продукты, поставляемые `SupplierID` указано в строке запроса. Кто-то отправил непосредственно на эту страницу, тем не менее, не будет знать, что рис. 12 показаны продукты Tokyo Traders. Чтобы исправить это, мы можем отобразить сведения о поставщике на этой странице также.

Начните с добавления элемент управления FormView над элементом GridView продуктов. Создать новый элемент управления ObjectDataSource с именем `SuppliersDataSource` , вызывающий `SuppliersBLL` класса `GetSupplierBySupplierID(supplierID)` метод.


[![Выбор метода Getsuppliers() класса](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Рис. 13**: выберите `SuppliersBLL` класс ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![У элемента управления ObjectDataSource вызова метода GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Рис. 14**: иметь элемент управления ObjectDataSource вызывает `GetSupplierBySupplierID(supplierID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Как и в `ProductsBySupplierDataSource`, имеют *`supplierID`* параметру присвоено значение `SupplierID` значение строки запроса.


[![Заполнение supplierID значение параметра из значения строки запроса SupplierID](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Рис. 15**: заполнения *`supplierID`* значение параметра из `SupplierID` значение строки запроса ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image43.png))


При привязке FormView к ObjectDataSource в режиме конструктора, Visual Studio автоматически создаст FormView `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate` с элементами управления Label и веб-текстовое поле для каждого поля данных, возвращенных Элемент управления ObjectDataSource. Так как нам нужно только отобразить поставщика сведения можно удалить `InsertItemTemplate` и `EditItemTemplate`. Далее следует изменить ItemTemplate, чтобы он отображал название компании поставщика в `<h3>` элемент и адрес, Город, Страна и номер телефона под названием компании. Кроме того, можно вручную задать FormView `DataSourceID` и создайте `ItemTemplate` разметки, как делалось в "[отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" Учебник.

После внесения этих изменений декларативная разметка FormView должна выглядеть следующим:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Рис. 16 показана снимок экрана `ProductsForSupplierDetails.aspx` странице после информации о поставщике, описанные выше был включен.


[![Список продуктов включает сводку о поставщике](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Рис. 16**: список продуктов включает сводку о Supplier ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Последние касается для`ProductsForSupplierDetails.aspx`пользовательского интерфейса

Для улучшения пользователь интерфейс для этого отчета существует ряд дополнений, мы работаем для внесения `ProductsForSupplierDetails.aspx` страницы. В настоящее время единственным способом, пользователь может перейти от `ProductsForSupplierDetails.aspx` страницы обратно к списку поставщиков является нажмите кнопку "Назад" браузера. Давайте добавим элемент управления HyperLink для `ProductsForSupplierDetails.aspx` страницы, которая ссылается на `SupplierListMaster.aspx`, предоставляя другим способом для пользователя, чтобы вернуться к основному списку.


[![Добавление элемента управления HyperLink для возврата пользователя страниц SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Рис. 17**: Добавление элемента управления HyperLink для возврата пользователя к `SupplierListMaster.aspx` ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Если пользователь щелкает ссылку Просмотр продуктов для поставщика, который не имеет любых продуктов `ProductsBySupplierDataSource` ObjectDataSource в `ProductsForSupplierDetails.aspx` не вернет результаты. GridView, привязанный к ObjectDataSource не будет отображать разметку, полученный в пустую область страницы в браузере пользователя. Для более ясного сообщения пользователю и что нет ни одного продукта, связанного с выбранным поставщиком можно присвоить свойству `EmptyDataText` в сообщение, должно отображаться при возникновении такой ситуации свойство. Я установил этого свойства значение «Нет ни одного продукта, предоставляемые этой поставщиком»

По умолчанию все поставщики в базе данных northwinds поставляют предоставляют хотя бы один продукт. Тем не менее, в этом руководстве я изменил `Products` таблицы, чтобы поставщик Escargots Nouveaux больше не связан с продуктами. Рис. 18 показана страница подробностей для Escargots Nouveaux после внесения этого изменения.


[![Пользователи получат сообщение о том, что у поставщика отсутствуют продукты](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Рис. 18**: пользователи получат сообщение о том, что у поставщика отсутствуют продукты ([Просмотр полноразмерного изображения](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Сводка

Хотя основные и подробные записи в основных/подробных отчетах могут отображаться на одной странице, на многих веб-сайтах они размещены на двух веб-страницах. В этом учебнике мы рассмотрели способы реализация такого отчета «основной/подробности» посредством перечисления поставщиков в элементе управления GridView на «основной» веб-странице и соответствующих продуктов на странице «Подробности». Каждая строка поставщика на главной веб-странице содержит ссылку на страницу сведений, которая передается строки `SupplierID` значение. Такие ссылки определенных строк можно легко добавить с помощью поля HyperLinkField элемента GridView.

На странице подробностей Получение продуктов для указанного поставщика достигалось посредством вызова принадлежащего `ProductsBLL` класса `GetProductsBySupplierID(supplierID)` метод. *`supplierID`* Значение параметра было указано декларативно с помощью строки запроса как источник параметра. Мы также рассмотрели способы отображения сведений о поставщике на странице подробностей с помощью элемента FormView.

Наши [следующему руководству](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) является последним, посвященным отчетам «основной/подробности». Мы рассмотрим Отображение списка продуктов в элементе управления GridView, в котором каждая строка содержит кнопки "выбрать". При нажатии кнопки Select будут отображаться сведения о продукте в элементе управления DetailsView на этой же странице.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Hilton giesenow). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Вперед](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
