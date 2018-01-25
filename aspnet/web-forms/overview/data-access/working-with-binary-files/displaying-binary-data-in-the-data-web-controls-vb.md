---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: "Отображение двоичных данных в данных веб-элементов управления (Visual Basic) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим параметры для представления двоичных данных в веб-страницы, включая отображение файла изображения и предоставление ссылка «Загрузки» f..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: df79748bf5734ffcb9eb81ca089aeded0e63bdc5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Отображение двоичных данных в данных веб-элементов управления (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) или [скачать PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> В этом учебнике мы рассмотрим параметры для представления двоичных данных в веб-страницы, включая отображение файла изображения и предоставление ссылку «Загрузить» для PDF-файла.


## <a name="introduction"></a>Вступление

В предыдущем учебнике изучена два способа для сопоставления двоичных данных с s приложения базовую модель данных и используется для передачи файлов из браузера в файловой системе web server s FileUpload управления. Мы хранять еще, чтобы увидеть как связать отправленного двоичных данных с моделью данных. То есть после файл загружен и сохранен в файловой системе, путь к файлу должны храниться в соответствующей базе данных записи. Если данные хранятся непосредственно в базе данных, переданные двоичных данных нет необходимости сохранять в файловой системе, но должны вноситься в базе данных.

Прежде чем мы рассмотрим связь данных с моделью данных, однако позволяют s сначала посмотрим, как обеспечить двоичные данные для конечного пользователя. Презентация текстовых данных является довольно простой, но как двоичные данные будут видеть? Он зависит, разумеется, тип данных binary. Для изображения скорее всего нужно для отображения изображения. для PDF-файлов документов Microsoft Word, ZIP-файлов и других типов двоичных данных, предоставляя ссылку для загрузки, возможно, более подходящим.

В данном руководстве, мы рассмотрим способы представления двоичных данных вместе с его данными связанный текст с использованием данных веб-элементов управления GridView и DetailsView. В следующем уроке мы перейдем поступившими сопоставления загруженного файла с базой данных.

## <a name="step-1-providingbrochurepathvalues"></a>Шаг 1: Предоставление`BrochurePath`значения

`Picture` Столбца в `Categories` таблица уже содержит двоичные данные для различных категорий изображений. В частности `Picture` столбца для каждой записи содержится двоичное содержимое зернистостью, низкое качество, 16 цветов растрового изображения. Каждое изображение категории — 172 пикселей в ширину и 120 пикселей и занимает примерно 11 КБ. Какие дополнительные s, двоичное содержимое в `Picture` столбец содержит 78-байтовое [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) заголовок, который необходимо извлечь перед отображением изображения. Эта информация заголовка отсутствует, поскольку у базы данных Northwind корнями в Microsoft Access. В режиме доступа двоичные данные хранятся с использованием типа данных объекта OLE, который вещам на этот заголовок. На данном этапе будет показано, как разделение заголовков из этих низкое качество изображений для отображения на рисунке. В будущем учебника мы выполним сборку интерфейс для обновления категории s `Picture` столбца и заменить эти растровых изображений, которые используются заголовки OLE с эквивалентный изображений JPG без ненужных заголовков OLE.

В предыдущем учебнике мы узнали, как использовать элемент управления FileUpload. Таким образом можно пойти дальше и добавить брошюр файлы в файловой системе web server s. Таким образом, однако не обновляет `BrochurePath` столбца в `Categories` таблицы. В следующем уроке мы рассмотрим, как это можно сделать, но теперь нам необходимо вручную указать значения для этого столбца.

В этом учебнике s вы найдете семь брошюр PDF-файлов в `~/Brochures` папку, по одному для каждой из категорий, за исключением Морепродукты. Я намеренно пропущен, добавление Морепродукты брошюр для демонстрации обработки сценариев, где не все записи связаны двоичные данные. Чтобы обновить `Categories` таблица со следующими значениями, щелкните правой кнопкой мыши `Categories` узла из обозревателя серверов и выберите Показать таблицу данных. Затем укажите виртуальные пути к файлам брошюр для каждой категории, имеющий брошюр, как показано на рис. 1. Так как не брошюр для категории Морепродукты, оставьте его `BrochurePath` значение столбца s в виде `NULL`.


[![Вручную введите значения в столбце категорий таблицы s BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Рис. 1**: вручную введите значения для `Categories` таблицу s `BrochurePath` столбца ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Шаг 2: Предоставление ссылку для загрузки для брошюр в элементе управления GridView

С `BrochurePath` значений, заданных для `Categories` таблицы, мы готов для создания элемента управления GridView, перечислены в каждой категории, а также ссылка для загрузки брошюр категории s. На шаге 4 мы расширим этот GridView также отображать изображение категории s.

Запуск, перетащив элемент управления GridView с панели элементов в конструктор `DisplayOrDownloadData.aspx` страницы в `BinaryData` папки. Набор GridView s `ID` для `Categories` и через смарт-тег s GridView, привяжите его к новому источнику данных. В частности, привяжите его к элементу ObjectDataSource с именем `CategoriesDataSource` получает данные с помощью `CategoriesBLL` объект s `GetCategories()` метод.


[![Создать новый элемент управления ObjectDataSource CategoriesDataSource с именем](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**На рисунке 2**: создать новый именованный ObjectDataSource `CategoriesDataSource` ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Настройка ObjectDataSource с помощью класса CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Рис. 3**: Настройка ObjectDataSource для использования `CategoriesBLL` класса ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Получить список категорий, с помощью метода GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Рис. 4**: получить список из категории с помощью `GetCategories()` метод ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


После завершения работы мастера настройки источника данных Visual Studio автоматически добавит поле BoundField для `Categories` GridView для `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, и `BrochurePath` `DataColumn` s. Продолжить и удалить `NumberOfProducts` BoundField с момента `GetCategories()` запроса метода s не получить эту информацию. Также удалите `CategoryID` BoundField и переименуйте `CategoryName` и `BrochurePath` стояли `HeaderText` свойства в категории и брошюр, соответственно. После внесения этих изменений, к GridView и ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Открыть эту страницу с помощью браузера (см. рис. 5). Отображается каждая из восьми категорий. Семь категорий с `BrochurePath` значения имеют `BrochurePath` значению, отображаемому в соответствующих BoundField. Морепродукты, имеющая `NULL` значение для его `BrochurePath`, отображает пустую ячейку.


[![Перечисленные каждой категории s имя, описание и значение BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Рис. 5**: s каждой категории имя, описание, и `BrochurePath` перечислены значения ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Вместо вывода текста `BrochurePath` столбец, необходимо создать ссылку на брошюр. Чтобы сделать это, удалите `BrochurePath` BoundField и замените его HyperLinkField. Задайте новый s HyperLinkField `HeaderText` свойства брошюр, его `Text` брошюр представление, свойства и его `DataNavigateUrlFields` свойства `BrochurePath`.


![Добавление HyperLinkField для BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Рис. 6**: HyperLinkField для добавления`BrochurePath`


Это добавит столбец ссылок к GridView, как показано на рис. 7. Щелчком по ссылке брошюр представления будут отображаться PDF-ФАЙЛ непосредственно в браузере или запрашивать пользователя, чтобы загрузить файл, в зависимости от того, установлено ли средство чтения PDF и параметры браузера s.


[![Можно просмотреть, щелкнув ссылку брошюр представление брошюр s категории](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Рис. 7**: s категории брошюр можно просмотреть, щелкнув ссылку брошюр представление ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Отображается категория s брошюр PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Рис. 8**: s категории брошюр PDF отображается ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Скрытие текста брошюр представления для категорий, без брошюр

Как показано на рис. 7, `BrochurePath` HyperLinkField отображает его `Text` значение свойства (представления брошюр) для всех записей, независимо от того, следует ли там s значение, отличное от`NULL` значение `BrochurePath`. Конечно Если `BrochurePath` — `NULL`, ссылка отображается как текст, а затем как в случае с категорией Морепродукты (указывают назад на рис. 7). Вместо отображения текста представления брошюр, может быть полезно обеспечить категорий без `BrochurePath` значение отображается альтернативный текст, как брошюр недоступно.

Чтобы обеспечить это поведение, необходимо использовать TemplateField, содержимое которого создается путем вызова метода страницы, который создает соответствующие выходные данные на основе `BrochurePath` значение. Мы сначала изучена это форматирование методика обратно в [с помощью TemplateFields в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) учебника.

Чтобы включить HyperLinkField в TemplateField, установите `BrochurePath` HyperLinkField и щелкнув преобразовать это поле в TemplateField ссылку в диалоговом окне Правка столбцов.


![Преобразование HyperLinkField в TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Рис. 9**: преобразование HyperLinkField в TemplateField


Это создаст TemplateField с `ItemTemplate` , содержащую проект гиперссылку веб-элемент управления `NavigateUrl` привязано свойство `BrochurePath` значение. Эта разметка замените вызов метода `GenerateBrochureLink`, передавая значение `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Создайте `Protected` метод в ASP.NET страница с именем класса с выделенным кодом s `GenerateBrochureLink` , возвращающий `String` и принимает `Object` как входной параметр.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Этот метод определяет, если переданный `Object` значение — это база данных `NULL` и, если это так, возвращает сообщение, указывающее, что категория отсутствуют брошюр. В противном случае, если имеется `BrochurePath` значение, он s, отображаемых в гиперссылку. Обратите внимание, что если `BrochurePath` значение представить его s, переданные в [ `ResolveUrl(url)` метод](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Этот метод разрешает переданный *URL-адрес*, заменив `~` символ с соответствующий виртуальный путь. Например, если приложение находится в папке `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` вернет `/Tutorial55/Brochures/Meat.pdf`.

Рис. 10 показана страница после внесения этих изменений. Обратите внимание, что категория Морепродукты s `BrochurePath` поле теперь отображает текст брошюр недоступно.


[![Брошюр недоступно текст отображается для тех категорий без брошюр](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Рис. 10**: отображаемый текст брошюр недоступно для тех категорий без брошюр ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Шаг 3: Добавление веб-страницы для отображения рисунков категории s

Когда пользователь открывает страницу ASP.NET, они получают s HTML страницы ASP.NET. Получено HTML только текст и не содержит любые двоичные данные. Любые дополнительные двоичные данные, такие как изображения, звуковые файлы, приложения Macromedia Flash, внедренные видео проигрыватель Windows Media и т. д. существуют как отдельные ресурсы на веб-сервере. HTML-код содержит ссылки на эти файлы, но не включает фактическое содержимое файлов.

Например, в формате HTML `<img>` элемент используется для ссылки с изображением, `src` атрибут, указывающий на файл изображения следующим образом:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Когда браузер получает следующий код HTML, он выполняет другой запрос к веб-серверу для получения двоичного содержимого файла изображения, которые затем отображаются в браузере. Также сохраняет любые двоичные данные. На шаге 2 брошюр не было отправлено в браузер как часть s HTML-разметку страницы. Вместо этого отображаемого HTML-кода представлена гиперссылки, после щелчка за браузер для запроса непосредственно в документ PDF.

Чтобы отобразить или разрешить пользователям загрузку двоичных данных, который находится в базе данных, необходимо создать отдельный веб-страницы, которое возвращает данные. Для нашего приложения существует s хранятся непосредственно в базы данных s категории рисунок только одно поле двоичных данных. Таким образом, нам нужно страницы, при вызове возвращает данные изображения для определенной категории.

Добавьте новую страницу ASP.NET в `BinaryData` папку с именем `DisplayCategoryPicture.aspx`. При этом использовать главную страницу установить флажок. Эта страница ожидает `CategoryID` значения в строки запроса и возвращает двоичные данные из этой категории s `Picture` столбца. Так как эта страница возвращает двоичные данные и никакие другие, не обязательно разметку в разделе HTML. Таким образом, нажмите на вкладке "источник" в левом нижнем углу и удалите все разметки страницы s, за исключением `<%@ Page %>` директивы. То есть `DisplayCategoryPicture.aspx` s декларативная разметка должна состоять из одной строки:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Если вы видите `MasterPageFile` атрибута в `<%@ Page %>` директивы, удалите его.

В классе окна кода добавьте следующий код для `Page_Load` обработчик событий:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Этот код запускает посредством считывания `CategoryID` значение строки запроса в переменную с именем `categoryID`. Далее рисунок данные извлекаются через вызов `CategoriesBLL` класса s `GetCategoryWithBinaryDataByCategoryID(categoryID)` метод. Эти данные возвращаются клиенту с помощью `Response.BinaryWrite(data)` метода, но до вызова этого `Picture` заголовка OLE значение s столбца должны быть удалены. Это достигается путем создания `Byte` массив с именем `strippedImageData` , будет содержать точно 78 символов меньше, чем возможности `Picture` столбца. [ `Array.Copy` Метод](https://msdn.microsoft.com/library/z50k9bft.aspx) используется для копирования данных из `category.Picture` начиная с позиции 78 через для `strippedImageData`.

`Response.ContentType` Указывает свойство [тип MIME](http://en.wikipedia.org/wiki/MIME) возврата, чтобы браузер умеет отображать его содержимого. Поскольку `Categories` таблицу s `Picture` столбца растрового изображения, растровое изображение, тип MIME здесь (изображения и bmp). Если не указан тип MIME, большинство браузеров по-прежнему выводится изображение правильно, так как они могут определять тип на основе содержимого двоичных данных s файла изображения. Тем не менее он s, разумным шагом будет включать MIME типа по возможности. В разделе [веб-сайта Internet Assigned Numbers Authority s](http://www.iana.org/) полный список [типы мультимедиа MIME](http://www.iana.org/assignments/media-types/).

С этой страницы, можно просмотреть рисунок определенной категории s, посетив `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Рис. 11 показана Напитки категории s, картинка можно просмотреть в `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Категория напитков s, который изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Рис. 11**: s категория напитков изображения ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Если при посещении `DisplayCategoryPicture.aspx?CategoryID=categoryID`две вещи, которые могут быть причиной этого, вы получаете исключение, которое считывает не удалось привести объект типа «System.DBNull» к типу «System.Byte []». Во-первых, `Categories` таблицу s `Picture` столбец допускает `NULL` значения. `DisplayCategoryPicture.aspx` Страницы, тем не менее, предполагает, что имеется значение, отличное от`NULL` имеется значение. `Picture` Свойство `CategoriesDataTable` невозможно открыть напрямую, если у него есть `NULL` значение. Если вы хотите разрешить `NULL` значения `Picture` столбец d необходимо включить следующее условие:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Приведенный выше код предполагает наличие s некоторых изображений в файл с именем `NoPictureAvailable.gif` в `Images` папку, которая будет отображаться для этих категорий без рисунка.

Это исключение также может быть вызвано, если `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` метод s `SELECT` инструкции вернулся списка столбцов основной запрос s может произойти, если вы используете специализированные инструкции SQL, и можно удалить, повторно запустите мастер для s адаптера таблицы основной запрос. Убедитесь, что `GetCategoryWithBinaryDataByCategoryID` метод s `SELECT` инструкции по-прежнему включает `Picture` столбца.

> [!NOTE]
> Каждый раз `DisplayCategoryPicture.aspx` — посещенные, имеет доступ к базе данных и возвращается указанной категории s данные изображения. Если рисунок изменялся категории s t изменен с момента пользователя последнего просмотрели ее, хотя это бесполезным. К счастью, HTTP позволяет *условного возвращает*. С помощью условного метода GET, клиент делает запрос HTTP отправляет вдоль [ `If-Modified-Since` заголовок HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , предоставляющий даты и времени, клиент последнего получения этот ресурс с веб-сервера. Если содержимое не изменилось, так как указанная дата, веб-сервер может вернуть [код состояния не изменено (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) и отказаться от отправляет содержимое запрошенный ресурс. Иными словами этот метод сбрасывает без отправки содержимого для ресурса, если он не был изменен с момента последнего обращения к клиентам его веб-сервере.


Для реализации этого поведения, тем не менее, необходимо добавить `PictureLastModified` столбец `Categories` таблицу для записи, когда `Picture` последнего обновления столбца и код для проверки `If-Modified-Since` заголовок. Дополнительные сведения о `If-Modified-Since` заголовок и условном рабочем процессе GET см [Conditional GET HTTP для RSS хакеров](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) и [глубоко рассмотреть выполнение запросов HTTP на странице ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Шаг 4: Отображение категорий изображения в элементе управления GridView

Теперь, когда у нас есть веб-страницы для отображения рисунков определенной категории s, можно отобразить с помощью [изображение веб-элемента управления](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) или HTML `<img>` элемент, указывающий на `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Базы данных определяется, URL-адрес изображения могут отображаться в GridView или с помощью ImageField DetailsView. Содержит ImageField `DataImageUrlField` и `DataImageUrlFormatString` свойства, которые работают как HyperLinkField s `DataNavigateUrlFields` и `DataNavigateUrlFormatString` свойства.

Разрешить s дополнять `Categories` GridView в `DisplayOrDownloadData.aspx` путем добавления ImageField отображения каждого рисунка категории s. Просто добавьте ImageField и задайте его `DataImageUrlField` и `DataImageUrlFormatString` свойства `CategoryID` и `DisplayCategoryPicture.aspx?CategoryID={0}`соответственно. Это создаст столбец GridView, отображающий `<img>` элемент которого `src` ссылки на атрибуты `DisplayCategoryPicture.aspx?CategoryID={0}`, где {0} заменяется строки GridView s `CategoryID` значение.


![Добавить ImageField в GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Рис. 12**: Добавление ImageField GridView


После добавления ImageField, декларативный синтаксис s GridView должна выглядеть, как soothe следующие:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Теперь пора просматривать эту страницу через браузер. Обратите внимание на то, как каждая запись теперь включает рисунка для категории.


[![Категория s рисунок отображается для каждой строки.](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Рис. 13**: s категории изображения для каждой строки ([Просмотр полноразмерное изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели, как представление двоичных данных. Способ представления данных зависит от типа данных. Для брошюр PDF-файлов, мы предоставляется брошюр представление связи, при щелчке заняло пользователь PDF-файл. Для рисунка s категории сначала создать страницу, чтобы извлечь и вернуть двоичные данные из базы данных и затем использовать эту страницу для отображения в элементе управления GridView рисунка s каждой категории.

Теперь, мы хранять изучали Вывод двоичных данных, мы готов к рассмотрению выполнения вставки, обновления и удаления в базе данных с двоичным данным. В следующем уроке мы рассмотрим связывание загруженного файла с его соответствующей записи базы данных. После этого учебника вы узнаете, как обновить существующие двоичные данные, а также по удалению двоичных данных, при удалении его соответствующей записи.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Мерфи Тереза д и Dave Gardner. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](uploading-files-vb.md)
[Вперед](including-a-file-upload-option-when-adding-a-new-record-vb.md)
