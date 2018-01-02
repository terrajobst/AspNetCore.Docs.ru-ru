---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: "Фильтрация на двух страницах (VB) иерархического | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы Реализуйте этот шаблон с помощью GridView, чтобы вывести список поставщиков в базе данных. Каждая строка поставщика в GridView, будет содержать Vie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: c34476f89677fb51abc17bd64602c41dfea8f9c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Иерархического фильтрации на двух страницах (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) или [скачать PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> В этом учебнике мы Реализуйте этот шаблон с помощью GridView, чтобы вывести список поставщиков в базе данных. Каждая строка поставщика в GridView будет содержать ссылку Просмотр продуктов, если он выбран, чтобы начать на отдельную страницу, в которой перечислены эти продукты для выбранного поставщика.


## <a name="introduction"></a>Вступление

На предыдущих двух уроках мы узнали, как отображать отчеты главного и подчиненного представлений одной веб-странице с помощью элементами управления DropDownList для отображения записей «главный» и [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) или [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) элемента управления для отображения» Подробности». Другой распространенный подход используется для основной подробности отчетов является основных записей в одной веб-страницы и сведения, указанные на другом. Форум по веб-сайта, таких как [форумы ASP.NET](https://forums.asp.net/), является хорошим примером того, этот шаблон, на практике. Форумы ASP.NET состоят из различных форумов Приступая к работе, веб-формы, элементы управления представления данных и т. д. Форум по каждой состоит из нескольких потоков, и каждый поток состоит из нескольких записей. На домашней странице форумов ASP.NET перечислены форумах. Щелкнув на форуме перейти к `ShowForum.aspx` страницы, который содержит список потоков для этого форума. Аналогичным образом, щелкнув в потоке, можно перейти к `ShowPost.aspx`, которая отображает сообщения для потока, который был щелкнут.

В этом учебнике мы Реализуйте этот шаблон с помощью GridView, чтобы вывести список поставщиков в базе данных. Каждая строка поставщика в GridView будет содержать ссылку Просмотр продуктов, если он выбран, чтобы начать на отдельную страницу, в которой перечислены эти продукты для выбранного поставщика.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Шаг 1: Добавление`SupplierListMaster.aspx`и`ProductsForSupplierDetails.aspx`страниц к`Filtering`папки

При определении макета страницы в третьем учебнике мы добавили несколько страниц «начальный» в `BasicReporting`, `Filtering`, и `CustomFormatting` папки. Тем не менее, мы был не добавлен начальной страницы для этого учебника в это время, поэтому уделите некоторое время, чтобы добавить новую страницу, чтобы `Filtering` папки: `SupplierListMaster.aspx` и `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx`Отображает записи «главный» (поставщики) при `ProductsForSupplierDetails.aspx` будет отображать продукты для выбранного поставщика.

При создании этих двух новых страниц обязательно связать их с `Site.master` главной страницы.


![Добавление SupplierListMaster.aspx и ProductsForSupplierDetails.aspx страницы в папку фильтрации](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Рис. 1**: добавление `SupplierListMaster.aspx` и `ProductsForSupplierDetails.aspx` страниц к `Filtering` папки


Кроме того, при добавлении новых страниц в проект, необходимо обновить файл карты сайта, `Web.sitemap`, соответствующим образом. В этом учебнике созданы `SupplierListMaster.aspx` страницы для карты сайта, используя следующее содержимое XML как дочерний для фильтрации отчетов `<siteMapNode>` элемента:


[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Вы можете помочь автоматизировать процесс обновления файла карты узла, при добавлении нового ASP.NET страниц с помощью [к. Скотт Аллен](http://odetocode.com/Blogs/scott/)Освободите Visual Studio [макрос карты сайта](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Шаг 2: Отображение списка поставщиков в`SupplierListMaster.aspx`

С `SupplierListMaster.aspx` и `ProductsForSupplierDetails.aspx` страницы, созданные, следующим шагом является создание элемента управления GridView для поставщиков из `SupplierListMaster.aspx`. Добавление элемента управления GridView на страницу и привязать его к новой ObjectDataSource. Следует использовать этот элемент управления ObjectDataSource `SuppliersBLL` класса `GetSuppliers()` для возврата всех поставщиков.


[![Выберите класс SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**На рисунке 2**: выберите `SuppliersBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image4.png))


[![Настройка ObjectDataSource можно использовать метод GetSuppliers()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Рис. 3**: Настройка ObjectDataSource для использования `GetSuppliers()` метод ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image7.png))


Нам нужно включить ссылку под названием Просмотр продуктов в каждой строке GridView, после щелчка направляет пользователя на `ProductsForSupplierDetails.aspx` передачи в выделенной строке `SupplierID` значение с помощью строки запроса. Например, если пользователь щелкает ссылку Просмотр продуктов для поставщика Tokyo Traders (которая имеет `SupplierID` значение 4), они должны отправляться `ProductsForSupplierDetails.aspx?SupplierID=4`.

Для этого добавьте [HyperLinkField](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.hyperlinkfield.aspx) к GridView, который добавляет гиперссылки к каждой строке GridView. Запустить, щелкнув ссылку Правка столбцов GridView смарт-теге. Затем выберите из списка в верхнем левом углу HyperLinkField и нажмите кнопку Добавить, чтобы включить HyperLinkField в списке полей в GridView.


[![Добавить HyperLinkField в GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Рис. 4**: Добавление GridView HyperLinkField ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image10.png))


HyperLinkField могут быть настроены для использования того же текста или URL-адрес значения по ссылке в каждой строке GridView, или эти значения могут быть основаны на значения данных, привязанный к каждой конкретной строки. Чтобы указать статическое значение для всех строк используйте HyperLinkField `Text` или `NavigateUrl` свойства. Поскольку мы хотим текст ссылки, должны совпадать для всех строк, значение HyperLinkField `Text` свойства для просмотра продуктов.


[![Значение свойства Text HyperLinkField Просмотр продуктов](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Рис. 5**: задать HyperLinkField `Text` свойства для просмотра продуктов ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image13.png))


Чтобы задать текст или URL-адрес на основе базовых данных, привязанных к строке GridView, укажите текст поля данных или значений URL-адреса должна извлекаться с в `DataTextField` или `DataNavigateUrlFields` свойства. `DataTextField`может устанавливаться только для одного поля данных; `DataNavigateUrlFields`, однако можно присвоить значение с разделителями запятыми список полей данных. Мы часто бывает необходимо базовый текст или URL-адреса на значение поля данных в текущей строке и некоторые разметки static. В этом учебнике, например, мы хотим URL-адрес ссылки HyperLinkField быть `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, где  *`supplierID`*  каждого GridView строка `SupplierID` значение. Обратите внимание, что нам нужно как статическим, так и управляемых данными значения здесь: `ProductsForSupplierDetails.aspx?SupplierID=` часть URL-адресе ссылки является статическим, тогда как  *`supplierID`*  часть является управляемой данными имеет его значение каждой строки собственные `SupplierID` значение.

Чтобы показать сочетания значений статических и управляемых данными, используйте `DataTextFormatString` и `DataNavigateUrlFormatString` свойства. В этих свойствах разметки static при необходимости введите и затем с помощью маркера `{0}` нужное значение поля, указанного в `DataTextField` или `DataNavigateUrlFields` свойства для отображения. Если `DataNavigateUrlFields` свойство имеет указанное использование нескольких полей `{0}` там, где требуется значение первого поля вставлены, `{1}` для второго значения поля и т. д.

Применения в этом руководстве, необходимо установить `DataNavigateUrlFields` свойства `SupplierID`, так как это поле данных, значение которого необходимо настроить на основе построчные и `DataNavigateUrlFormatString` свойства `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Настройка HyperLinkField включив правильную ссылку URL-адрес, основанный на SupplierID](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Рис. 6**: Настройка HyperLinkField для включения правильную ссылку URL-адрес на основе после `SupplierID` ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image16.png))


После добавления HyperLinkField, вы можете настроить и изменить порядок полей в GridView. В следующем примере показана GridView, после некоторые незначительные изменения уровня полей.


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Теперь пора просмотра `SupplierListMaster.aspx` страницу в обозревателе. Как показано на рис. 7, на странице в настоящее время перечисляются все поставщики, включая ссылку Просмотр продуктов. Щелкнув Просмотр продуктов ссылка ведет к `ProductsForSupplierDetails.aspx`, передав вдоль поставщика `SupplierID` в строке запроса.


[![Каждая строка поставщика содержит ссылку на представление продуктов](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Рис. 7**: каждая строка поставщика содержит ссылку на представление продуктов ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Шаг 3: Перечисление его товаров в`ProductsForSupplierDetails.aspx`

На этом этапе `SupplierListMaster.aspx` страницы отправляет пользователям `ProductsForSupplierDetails.aspx`, передав выбранного поставщика `SupplierID` в строке запроса. Последний шаг учебника является отображение продуктов в элементе управления GridView в `ProductsForSupplierDetails.aspx` которого `SupplierID` равняется `SupplierID` передаются в строке запроса. Для этого необходимо добавить GridView к `ProductsForSupplierDetails.aspx` страницы, используя новый элемент управления ObjectDataSource `ProductsBySupplierDataSource` , вызывающий `GetProductsBySupplierID(supplierID)` метод `ProductsBLL` класса.


[![Добавить новый элемент управления ObjectDataSource ProductsBySupplierDataSource с именем](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Рис. 8**: добавить новый элемент управления ObjectDataSource с именем `ProductsBySupplierDataSource` ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image22.png))


[![Выберите класс ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Рис. 9**: выберите `ProductsBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image25.png))


[![Вызовите метод GetProductsBySupplierID(supplierID) ObjectDataSource имеют](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Рис. 10**: У вызова ObjectDataSource `GetProductsBySupplierID(supplierID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image28.png))


На последнем этапе мастер настройки источника данных появляется запрос на указание источника `GetProductsBySupplierID(supplierID)` метода  *`supplierID`*  параметра. Чтобы использовать значение строки запроса, в качестве источника параметра строки запроса и введите имя значения строки запроса, используемого в текстовом поле QueryStringField (`SupplierID`).


[![Заполнение supplierID значение параметра из SupplierID значения строки запроса](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Рис. 11**: заполнение  *`supplierID`*  значение параметра из `SupplierID` значение строки запроса ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image31.png))


Это все, чтобы он! Рис. 12 показан `ProductsForSupplierDetails.aspx` страницы при посещении, щелкнув ссылку Tokyo Traders из `SupplierListMaster.aspx`.


[![Отображаются продукты предоставляемые Tokyo Traders](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Рис. 12**: отображаются продукты предоставляемые Tokyo Traders ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Отображение сведений о поставщике в`ProductsForSupplierDetails.aspx`

Как показано на рис. 12, `ProductsForSupplierDetails.aspx` странице просто перечислены продукты, поставляемые `SupplierID` указано в строке запроса. Кто-то отправил непосредственно на эту страницу, тем не менее, не будет знать, что на рис. 12 показан Tokyo Traders продуктов. Чтобы устранить эту на этой странице также можно отобразить сведения о поставщиках.

Начните с добавления FormView выше продукты GridView. Создать новый элемент управления ObjectDataSource `SuppliersDataSource` , вызывающий `SuppliersBLL` класса `GetSupplierBySupplierID(supplierID)` метод.


[![Выберите класс SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Рис. 13**: выберите `SuppliersBLL` класса ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image37.png))


[![Вызовите метод GetSupplierBySupplierID(supplierID) ObjectDataSource имеют](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Рис. 14**: У вызова ObjectDataSource `GetSupplierBySupplierID(supplierID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image40.png))


Как и в `ProductsBySupplierDataSource`, имеют  *`supplierID`*  параметру присвоено значение `SupplierID` значение строки запроса.


[![Заполнение supplierID значение параметра из SupplierID значения строки запроса](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Рис. 15**: заполнение  *`supplierID`*  значение параметра из `SupplierID` значение строки запроса ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image43.png))


При привязке FormView ObjectDataSource в представлении конструирования, Visual Studio автоматически создаст FormView `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate` с помощью элементов управления Label и веб-текстовое поле для каждого поля данных, возвращенных ObjectDataSource. Поскольку необходимо отобразить supplier сведения смело удалить `InsertItemTemplate` и `EditItemTemplate`. Теперь измените ItemTemplate, чтобы оно отображалось название компании в `<h3>` элемент и адреса, города, страны и номер телефона под название компании. Кроме того, можно вручную задать FormView `DataSourceID` и создать `ItemTemplate` разметки, как это делалось в «[отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)» учебника.

После этого разметка FormView должна выглядеть следующим образом:


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Снимок экрана показано на рисунке 16 `ProductsForSupplierDetails.aspx` после того, как сведения о поставщиках, описанные выше был включен.


[![Список продуктов содержит сводные данные о поставщике](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**На рисунке 16**: список продуктов содержит сводные данные о других поставщиков ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Применение конечное касается для`ProductsForSupplierDetails.aspx`пользовательского интерфейса

Для улучшения пользователь взаимодействие для этого отчета существует на ряд дополнений, мы работаем внести `ProductsForSupplierDetails.aspx` страницы. В настоящее время единственным способом, пользователь может перейти от `ProductsForSupplierDetails.aspx` страницы обратно в список поставщиков, щелкнув их кнопку Назад». Давайте добавим элемент управления гиперссылки для `ProductsForSupplierDetails.aspx` страницы, которая ссылается на `SupplierListMaster.aspx`, предоставляя другим способом для пользователя, чтобы вернуться в главном списке.


[![Добавление элемента управления HyperLink, чтобы перенаправить пользователя SupplierListMaster.aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Рисунок 17**: Добавление элемента управления HyperLink предпринять пользователя обратно к `SupplierListMaster.aspx` ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image49.png))


Если пользователь щелкает ссылку Просмотр продуктов для поставщика, который не содержит какие-либо продукты `ProductsBySupplierDataSource` ObjectDataSource в `ProductsForSupplierDetails.aspx` не возвращает никаких результатов. GridView, связанного с элементом ObjectDataSource не создают разметку возникающие в пустую область страницы в браузере. Для более четкого взаимодействие с пользователем, что нет ни одного продукта, связанных с выбранной поставщика можно присвоить свойству `EmptyDataText` свойства сообщения, мы должен отображаться, когда возникает такая ситуация. Я установил этого свойства значение «Нет ни одного продукта, предоставляемые поставщика»

По умолчанию для всех поставщиков в базе данных Northwinds предоставляют по крайней мере одного продукта. Однако в этом учебнике я вручную изменил `Products` таблицы, чтобы поставщик Escargots Nouveaux больше не связана с продуктами. На рисунке 18 показана страница сведений для Escargots Nouveaux после внесения этого изменения.


[![Пользователи получат сообщение о том, что поставщик не предоставляет какие-либо продукты](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**На рисунке 18**: пользователи получат сообщение о том, что поставщик не предоставляет какие-либо продукты ([Просмотр полноразмерное изображение](master-detail-filtering-across-two-pages-vb/_static/image52.png))


## <a name="summary"></a>Сводка

Хотя отчеты главного и подчиненного представлений можно отобразить основные и подробные записи на одной странице, на многих веб-сайтах они размещены на двух веб-страниц. В этом учебнике мы рассмотрели реализации такого отчета главного и подчиненного представлений с с поставщиками, перечисленные в элементе управления GridView в веб-странице «главный», а также связанные продукты на странице «Подробности». Каждая строка поставщика в главной веб-страницы содержится ссылка на страницу сведений, который передается строка `SupplierID` значение. Такие специфические для строки ссылки легко добавить с помощью HyperLinkField GridView.

На странице сведений о получении этих продуктов для указанного поставщика выполнялось с помощью вызова `ProductsBLL` класса `GetProductsBySupplierID(supplierID)` метод.  *`supplierID`*  Было указано значение параметра, декларативно с помощью строки запроса в качестве источника параметра. Также мы рассмотрели способ отображения сведений поставщика сведения о странице, с помощью FormView.

Наш [следующее руководство](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) является последний раздел на главного и подчиненного представлений отчетов. Мы рассмотрим Отображение списка продуктов в GridView, где каждая строка содержит кнопку выбора. Кнопки "Select" отображаются сведения о продукте в элементе управления DetailsView на одной странице.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](master-detail-filtering-with-two-dropdownlists-vb.md)
[Вперед](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
