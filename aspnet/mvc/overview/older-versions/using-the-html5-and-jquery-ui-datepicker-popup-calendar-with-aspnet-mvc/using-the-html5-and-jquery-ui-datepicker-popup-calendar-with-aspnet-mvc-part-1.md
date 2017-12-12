---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 1 | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 9320c8a2aadb3b3c5bd6cd90b59d8a72db384c0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 1
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в приложении MVC ASP.NET.


Этот учебник поможет узнать основные принципы работы с редактором шаблонов, шаблоны отображения и jQuery [раскрывающегося календаря с выбором дат пользовательского интерфейса](http://plugins.jquery.com/project/datepicker) в приложении MVC ASP.NET. В этом учебнике можно использовать Microsoft Visual Web Developer 2010 Express пакетом обновления 1 (&quot;Visual Web Developer&quot;), которое представляет бесплатную версию Microsoft Visual Studio, или если у вас уже есть, можно использовать Visual Studio 2010 с пакетом обновления 1.

Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Также можно отдельно установить необходимое программное обеспечение, используя следующие ссылки:

- [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)

Если вы используете Visual Studio 2010 вместо Visual Web Developer, установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

В этом учебнике предполагается завершения [Приступая к работе с MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) учебник или что вы знакомы с разработкой приложений ASP.NET MVC. Этот учебник начинается с завершенный проект из [Приступая к работе с MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) учебника.

В этом учебнике показано кода на языке C#. Тем не менее [начальный проект](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) и завершенного проекта также доступны в Visual Basic.

Проект Visual Studio с исходным кодом C# и Visual Basic доступна по следующему адресу: [загрузить](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Что мы создадим

Вы добавите шаблоны (в частности, редактировать и отображать шаблоны) для простого приложения список фильмов, который был создан в [Приступая к работе с MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) учебника. Необходимо добавить [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) раскрывающегося календаря, чтобы упростить процесс ввода даты. На следующем рисунке показан измененный приложения с отображаемого календаря всплывающего окна выбора дат jQuery пользовательского интерфейса.

![по завершении jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Ниже приведен изучаемого материала.

- Использование атрибутов из [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) пространство имен для контроля формата данных, при отображении и когда она находится в режиме редактирования.
- Создание шаблонов (изменение и отображение шаблонов) для управления форматированием данных.
- Добавление [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) образом, чтобы ввести полей дат.

### <a name="getting-started"></a>Начало работы

Если у вас еще нет приложения список фильмов из начального проекта, загрузите ее с помощью следующей ссылки: [загрузить](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). Затем в проводнике Windows щелкните правой кнопкой мыши *MvcMovie.zip* файла и выберите **свойства**. В **свойства MvcMovie.zip** выберите **Unblock**. (Разблокировке предотвращает предупреждение системы безопасности, которая возникает при попытке использования *.zip* файл, загруженный из Интернета.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Щелкните правой кнопкой мыши *MvcMovie.zip* файла и выберите **извлечь все** для распаковки файла. В Visual Studio 2010 или Visual Web Developer откройте *MvcMovieCS\_TU.sln* файла.

В **обозревателе решений**, дважды щелкните *представления\общие\\_Layout.cshtml* для его открытия. Изменение `H1` заголовок из **приложения MVC фильма** для **фильма jQuery**. Нажмите клавиши CTRL + F5, чтобы запустить приложение и нажмите кнопку **Главная** вкладки, которая понадобится для `Index` метод контроллера фильма. Чтобы проверить приложение, выберите **изменить** ссылку и **сведения** ссылку для одного из фильмов. Обратите внимание, что в индексе, Правка, и хорошо форматирования представления сведений, даты выпуска и Цена:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Форматирование даты и ценой является результатом использования [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибута в свойствах `Movie` класса.

Откройте *Movie.cs* файла и закомментируйте `DisplayFormat` атрибут `ReleaseDate` и `Price` свойства. Итоговый `Movie` класса выглядит следующим образом:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Нажмите клавиши CTRL + F5, еще раз, чтобы запустить приложение и выберите **Главная** вкладку, чтобы просмотреть список фильмов. На этот раз Дата выпуска показывает дату и время, а поле «цена» больше не отображается символ валюты. Изменения в `Movie` класса отменила хорошо форматирование, которое вы уже видели раньше, но это будет исправлено позже.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>С помощью атрибутов DataAnnotations DataType для указания типа данных

Замените комментарий `DisplayFormat` для атрибута `ReleaseDate` свойство с [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) атрибута с помощью `Date` перечисления. Замените `DisplayFormat` для атрибута `Price` свойство с [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) атрибут, это время с помощью `Currency` перечисления. Вот как выглядит готового кода:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Запустите приложение. Теперь дата выпуска и свойства цены форматируются правильно (то есть с помощью соответствующих форматов дат и валют). [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) атрибута предоставляет тип метаданных для встроенных ASP.NET MVC шаблоны, чтобы отображать поля в правильном формате. С помощью `DataType` атрибут предпочтительным является использование `DisplayFormat` атрибута, которое было изначально в коде, так как `DataType` атрибут делает модели более понятные и более гибкие, необходимого, например интернационализации.

В следующем разделе вы увидите, как создать пользовательские шаблоны для отображения полей даты.

>[!div class="step-by-step"]
[Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
