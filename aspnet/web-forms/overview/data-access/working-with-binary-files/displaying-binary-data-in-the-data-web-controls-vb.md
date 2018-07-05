---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Отображение двоичных данных в веб-данных управляет (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы рассмотрим параметры для представления двоичных данных на веб-странице, включая отображение файла изображения и предоставление f ссылку «Скачать»...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: a9d298ef328e951f235a6cfcd41b73fafefb0dfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373111"
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Отображение двоичных данных в данных веб-элементов управления (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) или [скачать PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> В этом руководстве мы рассмотрим параметры для представления двоичных данных на веб-странице, включая отображение файла изображения и предоставление ссылку «Скачать» для PDF-файла.


## <a name="introduction"></a>Вступление

В предыдущем учебном курсе мы изучили два различных способа для двоичных данных, связанной с s приложения базовую модель данных и позволяет передать файлы из браузера в файловой системе web server s элемента управления FileUpload. Мы ve еще для того, чтобы узнать, как должен быть сопоставлен отправленного двоичные данные в модель данных. То есть после передачи файла и сохранен в файловой системе, путь к файлу должен храниться в соответствующей базе данных записи. Если данные хранятся непосредственно в базе данных, отправленных двоичных данных нет необходимости сохранять в файловой системе, но необходимо вставлять события в базу данных.

Прежде чем мы рассмотрим связь данных с моделью данных, однако позволяют s сначала посмотрим, как обеспечить двоичные данные для конечного пользователя. Представления текстовых данных является достаточно простым, но следует двоичные данные представления? Он зависит, само собой, тип двоичных данных. Для образов скорее всего нужно вывести на изображения. для файлов PDF документы Microsoft Word, ZIP-файлы и другие виды двоичных данных, предоставляя ссылку для скачивания, вероятно, более подходящим.

В этом учебном курсе мы рассмотрим способ представления двоичных данных вместе с его данными связанный текст, с помощью данных веб-элементов управления GridView и DetailsView. В следующем учебном курсе мы обратим наше внимание сопоставления файлов, отправляемых с базой данных.

## <a name="step-1-providingbrochurepathvalues"></a>Шаг 1: Предоставление`BrochurePath`значения

`Picture` Столбца в `Categories` таблица уже содержит двоичные данные для различных категорий изображений. В частности `Picture` столбец для каждой записи содержит двоичное содержимое повышенную зернистость, низкого качества, 16 цветов точечного рисунка. Каждый образ категории — 172 пикселей в ширину и 120 пикселей и занимает приблизительно 11 КБ. Какие дополнительные s, двоичное содержимое в `Picture` столбца включает в себя 78 байтов [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) заголовок, который должен быть удалены перед отображением изображения. Эта информация заголовка присутствует, так как в базе данных "Борей" впервые появилась в Microsoft Access. В режиме доступа двоичных данных хранится с использованием тип данных объекта OLE, который добавляет к на этот заголовок. Сейчас узнаете, как удалить заголовки из эти изображения для отображения на рисунке. В следующем учебном курсе мы создадим интерфейс для обновления категории s `Picture` столбца и заменить эти точечных рисунков, которые используют заголовки OLE с эквивалентное изображения JPG без ненужных заголовки OLE.

В предыдущем учебном курсе мы рассмотрели использование элемента управления FileUpload. Таким образом можно пойти дальше и добавить буклет файлы в файловой системе web server s. Таким образом, однако не обновляет `BrochurePath` столбца в `Categories` таблицы. В следующем учебном курсе мы рассмотрим, как выполнить эту задачу, но сейчас нам нужно вручную указать значения для этого столбца.

В этом учебнике s вы найдете семь файлов PDF буклет в `~/Brochures` папки, по одному для каждой из категорий, за исключением морепродуктов. Я намеренно опущен, добавление буклета Морепродукты Демонстрация обработки сценариев, где не все записи связаны двоичных данных. Чтобы обновить `Categories` таблица со следующими значениями, щелкните правой кнопкой мыши `Categories` узла из обозревателя сервера и выберите Показать таблицу данных. Затем введите виртуальные пути к файлам буклет для каждой категории, имеющий буклета, как показано на рис. 1. Так как не буклет для категории морепродуктов, оставьте его `BrochurePath` значение столбца s в виде `NULL`.


[![Вручную введите значения в столбце категорий таблицы s BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Рис. 1**: вручную введите значения для `Categories` таблицы s `BrochurePath` столбца ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Шаг 2: Предоставление ссылку для скачивания для брошюр в элементе управления GridView

С помощью `BrochurePath` значения, предоставляемые для `Categories` таблицы, мы будет готов для создания элемента GridView, каждой категории, а также ссылку на загрузку брошюры категории s. На шаге 4 мы расширим этот GridView будет также отображаться изображение категории s.

Начнем с перетаскивания элемента управления GridView с панели инструментов в конструктор `DisplayOrDownloadData.aspx` странице в `BinaryData` папку. Набор GridView s `ID` для `Categories` и через s смарт-тега GridView, выберите привязать его к новому источнику данных. В частности, привязать его к элементу ObjectDataSource с именем `CategoriesDataSource` , получающий данные с помощью `CategoriesBLL` объект s `GetCategories()` метод.


[![Создайте новый ObjectDataSource, именуемый CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Рис. 2**: создайте новый ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Настройка ObjectDataSource на использование класса CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Рис. 3**: Настройка ObjectDataSource для использования `CategoriesBLL` класс ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Получение списка категорий с помощью метода GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Рис. 4**: получить список из категории с помощью `GetCategories()` метод ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


После завершения работы мастера настройки источников данных Visual Studio автоматически добавит поле BoundField в `Categories` GridView для `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, и `BrochurePath` `DataColumn` s. Продолжить и удалить `NumberOfProducts` BoundField с момента `GetCategories()` запроса метода s не получить эту информацию. Также удалите `CategoryID` BoundField и переименуйте `CategoryName` и `BrochurePath` поля BoundField, кроме `HeaderText` свойства в категории и брошюры, соответственно. После внесения этих изменений, к GridView и ObjectDataSource s декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Просмотреть эту страницу через обозреватель (см. рис. 5). Каждая из этих восьми категорий указан. Семь категорий с `BrochurePath` значения имеют `BrochurePath` значением, отображаемым в соответствующих BoundField. Морепродукты, имеющая `NULL` значение для его `BrochurePath`, отображает пустую ячейку.


[![Показаны все категории — имя, описание и значение BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Рис. 5**: Каждая категория — имя, описание, и `BrochurePath` перечислены значения ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Вместо вывода текста `BrochurePath` столбец, необходимо создать ссылку на брошюры. Чтобы выполнить это, удалите `BrochurePath` BoundField и замените его поля HyperLinkField. Задайте новое ре HyperLinkField `HeaderText` свойства брошюр, его `Text` брошюр представление, свойства и его `DataNavigateUrlFields` свойства `BrochurePath`.


![Добавление поля HyperLinkField для BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Рис. 6**: Добавление поля HyperLinkField для `BrochurePath`


Это будет добавлен столбец ссылок к GridView, как показано на рис. 7. Щелкните ссылку на представление буклет будет отображаться PDF-ФАЙЛ непосредственно в браузере или запрашивать пользователя, чтобы скачать файл, в зависимости от того, установлено ли средство чтения PDF и параметров браузера s.


[![Буклета s категории можно просмотреть, щелкнув ссылку буклет представления](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Рис. 7**: s категории буклет можно просмотреть, щелкнув ссылку Просмотр Буклет ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Категория s буклет PDF отображается](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Рис. 8**: категории s буклет PDF отображается ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Скрытие текста буклет представления для категорий без буклета

Как показано на рис. 7, `BrochurePath` HyperLinkField отображает его `Text` значение свойства (представление буклет) для всех записей, независимо от того, следует ли там s отличный от`NULL` значение `BrochurePath`. Само собой Если `BrochurePath` — `NULL`, то ссылка отображается в виде текста, как в случае с категории «Seafood» (см. рис. 7). Вместо вывода текст брошюры представление, было бы здорово иметь эти категории без `BrochurePath` значение отображения альтернативный текст, как доступные буклет нет.

Чтобы обеспечить такое поведение, нам нужно использовать поле TemplateField, содержимое которых создается путем вызова метода страницы, создает соответствующие выходные данные на основе `BrochurePath` значение. Мы сначала изучили это форматирование прием в [использование полей TemplateField в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) руководства.

Чтобы включить поле HyperLinkField в поле TemplateField, установите `BrochurePath` HyperLinkField и выбрав Convert это поле в TemplateField ссылку в диалоговом окне Правка столбцов.


![Преобразовать поле HyperLinkField в поле TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Рис. 9**: преобразовать поле HyperLinkField в поле TemplateField


Это создаст TemplateField с `ItemTemplate` , содержащее гиперссылку веб-узла со свойством `NavigateUrl` свойство привязано к `BrochurePath` значение. Замените эту разметку с помощью вызова метода `GenerateBrochureLink`, передавая значение `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Создайте `Protected` метод в ASP.NET страницы класса вспомогательного кода s с именем `GenerateBrochureLink` , возвращающий `String` и принимает `Object` как входной параметр.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Этот метод определяет, если переданный `Object` значение — это база данных `NULL` и, если это так, возвращает сообщение о том, что категория отсутствует буклета. В противном случае, если имеется `BrochurePath` значение, он, отображаемых в гиперссылку. Обратите внимание, что если `BrochurePath` значение — представить их s, переданные [ `ResolveUrl(url)` метод](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Этот метод разрешает переданный *URL-адрес*, заменив `~` символ с соответствующий виртуальный путь. Например, если приложение находится в папке `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` вернет `/Tutorial55/Brochures/Meat.pdf`.

Рис. 10 показана страница после внесения этих изменений. Обратите внимание, что категории морепродуктов s `BrochurePath` поле теперь отображает текст нет буклета доступны.


[![Текст нет буклет доступных отображается те категории без буклета](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Рис. 10**: области текста нет буклета доступны отображается те категории без буклета ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Шаг 3: Добавление веб-страницы для отображения рисунка категории s

При посещении пользователем страницы ASP.NET, они получают s HTML страницы ASP.NET. Полученный HTML только текст и не содержат любые двоичные данные. Любые дополнительные двоичные данные, такие как изображения, звуковые файлы, приложений Macromedia Flash, внедренные видео проигрывателя Windows Media и т. д. существуют как отдельные ресурсы на веб-сервере. HTML-код содержит ссылки на эти файлы, но не включает фактическое содержимое файлов.

Например, в формате HTML `<img>` элемент используется для ссылки на изображения, с помощью `src` атрибут, указывающий на файл изображения следующим образом:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Когда браузер получает следующий код HTML, он выполняет другой запрос на веб-сервер для получения двоичного содержимого файла изображения, которые затем отображаются в браузере. То же самое относится к любые двоичные данные. На шаге 2 брошюры не было отправлено в браузер как часть HTML-разметку страницы s. Вместо этого готовый для просмотра HTML-код представлена гиперссылок, при щелчке, вызвавшего браузер для запроса непосредственно в PDF-документе.

Для отображения или разрешить пользователям загрузку двоичных данных, который находится в базе данных, необходимо создать отдельный веб-страницы, которая возвращает данные. Для нашего приложения там s хранятся непосредственно в базу данных категории s рисунок только одно поле двоичных данных. Таким образом, нам нужно страницы, при вызове возвращает данные изображения для определенной категории.

Добавьте новую страницу ASP.NET к `BinaryData` папку с именем `DisplayCategoryPicture.aspx`. При этом выбрать главную страницу установить флажок. Эта страница ожидает `CategoryID` значение в строке запроса и возвращает двоичные данные из этой категории s `Picture` столбца. Так как эта страница возвращает двоичные данные и ничего более, любую разметку в разделе HTML не требуется. Таким образом, щелкните на вкладке "источник" в левом нижнем углу и удалить все разметки страницы s, за исключением `<%@ Page %>` директива. То есть `DisplayCategoryPicture.aspx` s декларативная разметка должна состоять из одной строки:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Если вы видите `MasterPageFile` атрибут в `<%@ Page %>` директивы, удалите его.

В классе фонового кода страницы s, добавьте следующий код, чтобы `Page_Load` обработчик событий:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Начинается этот код, считывая `CategoryID` значение строки запроса в переменную с именем `categoryID`. Затем извлекаются данные рисунок через вызов `CategoriesBLL` класс s `GetCategoryWithBinaryDataByCategoryID(categoryID)` метод. Эти данные возвращаются клиенту с помощью `Response.BinaryWrite(data)` метод, но прежде чем это называется `Picture` заголовка OLE значение s столбец должен быть удален. Это достигается путем создания `Byte` массив с именем `strippedImageData` , будет содержать точно 78 символов меньше, чем `Picture` столбца. [ `Array.Copy` Метод](https://msdn.microsoft.com/library/z50k9bft.aspx) используется для копирования данных из `category.Picture` начиная с позиции 78 через для `strippedImageData`.

`Response.ContentType` Указывает свойство [тип MIME](http://en.wikipedia.org/wiki/MIME) возвращаемых таким образом, чтобы обозреватель знает, как визуализировать его содержимого. Так как `Categories` таблицы s `Picture` столбца является точечным рисунком, растровое изображение, тип MIME здесь (изображение/bmp). Если не указан тип MIME, большинство браузеров будут по-прежнему отображаться изображение правильно так, как они может вывести тип, на основе содержимого образа файла s двоичных данных. Тем не менее его полезным включать MIME s введите по возможности. См. в разделе [веб-сайта Internet Assigned Numbers Authority](http://www.iana.org/) полный список [типах носителей MIME](http://www.iana.org/assignments/media-types/).

С этой страницей создан, можно просмотреть рисунок определенной категории s, посетив `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Рис. 11 показана рисунок категории s «Напитки», который можно просмотреть в `DisplayCategoryPicture.aspx?CategoryID=1`.


[![S категории «Напитки», появится изображение](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Рис. 11**: s категории «Напитки», изображения ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Если при просмотре `DisplayCategoryPicture.aspx?CategoryID=categoryID`, возникает исключение, которое считывает не удалось привести объект типа «System.DBNull» к типу «System.Byte []», есть две вещи, которые могут вызывать это. Во-первых, `Categories` таблицы s `Picture` столбец допускает `NULL` значения. `DisplayCategoryPicture.aspx` Страницы, тем не менее, предполагается, что имеется отличный от`NULL` имеется значение. `Picture` Свойство `CategoriesDataTable` невозможно открыть напрямую, если он имеет `NULL` значение. Если вы хотите разрешить `NULL` значений в параметре `Picture` столбце d необходимо включить следующее условие:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Приведенный выше код предполагает, что s некоторые изображений в файл с именем `NoPictureAvailable.gif` в `Images` папку, которая будет отображаться для этих категорий без рисунка.

Это исключение также может быть вызвано, если `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` метод s `SELECT` инструкции возвращена обратно к основного запроса s столбца списка, которая может произойти, если вы используете специализированные инструкции SQL и ve вы повторно запустите мастер для TableAdapter s основной запрос. Проверка того, что `GetCategoryWithBinaryDataByCategoryID` метод s `SELECT` инструкция по-прежнему содержит `Picture` столбца.

> [!NOTE]
> Каждый раз `DisplayCategoryPicture.aspx` будет посещен, базы данных доступна и возвращаются данные рисунка указанной категории s. Если изображение категории s t изменен с момента последнего просмотра пользователей, однако это затрачивать минимальные усилия. К счастью, HTTP позволяет *условного получает*. С помощью условного GET, клиент, посылающий запрос HTTP отправляет вдоль [ `If-Modified-Since` заголовок HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , предоставляющий дату и время, клиент последнего извлечения ресурс с веб-сервера. Если содержимое не изменилось, так как указанная дата, веб-сервер может отправить в ответ [код состояния не изменено (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) и отказ от отправляет содержимое запрошенный ресурс. Короче говоря этот метод освобождает обойтись без отправки содержимого для ресурса, если он не был изменен с момента последнего обращения к клиентам его веб-сервере.


Для реализации такого поведения, тем не менее, необходимо добавить `PictureLastModified` столбец `Categories` таблицу для фиксации, когда `Picture` а также код для проверки последнего обновления столбца `If-Modified-Since` заголовка. Дополнительные сведения о `If-Modified-Since` заголовка и условном рабочем процессе GET, см. в разделе [HTTP условного GET для хакеров RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) и [глубже рассмотрим выполнение HTTP-запросы на странице ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Шаг 4: Отображение изображений категории в элементе управления GridView

Теперь, когда у нас есть веб-страницы для отображения определенной категории s рисунка, можно отобразить с помощью [образа веб-элемента управления](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) или HTML `<img>` элемент, указывающий на `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Изображения, URL-адреса которых определяется базы данных могут отображаться в GridView и DetailsView, с помощью ImageField. Содержит ImageField `DataImageUrlField` и `DataImageUrlFormatString` свойства, которые работают как HyperLinkField s `DataNavigateUrlFields` и `DataNavigateUrlFormatString` свойства.

Позволяют дополнять s `Categories` GridView в `DisplayOrDownloadData.aspx` путем добавления ImageField отображения каждого рисунка категории s. Просто добавьте ImageField и задайте его `DataImageUrlField` и `DataImageUrlFormatString` свойства `CategoryID` и `DisplayCategoryPicture.aspx?CategoryID={0}`, соответственно. Это создаст столбец GridView, отображающий `<img>` элемент которого `src` ссылки на атрибуты `DisplayCategoryPicture.aspx?CategoryID={0}`, где {0} заменяется строке GridView s `CategoryID` значение.


![Добавить ImageField к GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Рис. 12**: Добавление ImageField к GridView


После добавления ImageField, декларативный синтаксис s GridView должна выглядеть, как soothe следующие:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Отвлекитесь и просмотреть эту страницу через обозреватель. Обратите внимание на то, как каждая запись теперь включает рисунка для категории.


[![Категория s рисунок отображается для каждой строки](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Рис. 13**: категории s изображения для каждой строки ([Просмотр полноразмерного изображения](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели способ представления двоичных данных. Способ представления данных зависит от типа данных. Для файлов PDF буклета, мы предлагали пользователь буклета представление связи, при нажатии заняло пользователя непосредственно в PDF-файл. Рисунка s категории мы сначала создали страницу, чтобы извлечь и вернуть двоичные данные из базы данных и затем используется этой страницы для отображения каждого изображения s категории в элементе управления GridView.

Теперь, мы ve изучали отображение двоичных данных, мы будет готов к рассмотрению для выполнения вставки, обновления и удаления в базе данных с двоичными данными. В следующем учебном курсе мы рассмотрим сопоставление файлов, отправляемых с его соответствующей записи базы данных. В этом руководстве, после этого мы рассмотрим, как обновить существующих двоичных данных, а также способ удаления двоичных данных, когда удаляется его связанную запись.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Терезой Мерфи и Дейв Gardner, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](uploading-files-vb.md)
> [Вперед](including-a-file-upload-option-when-adding-a-new-record-vb.md)
