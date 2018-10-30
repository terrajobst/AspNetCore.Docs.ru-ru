---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Использование HTML5 и элемент интерфейса всплывающего календаря jQuery в ASP.NET MVC. часть 1 | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник поможет основные сведения о работе с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: a4cd6e9adfcd85503b9843232903a243bc07c959
ms.sourcegitcommit: 392a36ed269b88899d6bb652aa7f4dfb72e43e7f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50220665"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Использование HTML5 и элемент интерфейса всплывающего календаря jQuery в ASP.NET MVC. часть 1
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> Этом учебнике описываются основы работы с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в приложении ASP.NET MVC.


Этот учебник поможет основные сведения о работе с помощью редактора шаблонов, шаблоны отображения и jQuery [элемент интерфейса всплывающего календаря](http://plugins.jquery.com/project/datepicker) в приложении ASP.NET MVC. Для этого руководства можно использовать Microsoft Visual Web Developer 2010 Express пакетом обновления 1 (&quot;Visual Web Developer&quot;), который является бесплатной версии Microsoft Visual Studio или Visual Studio 2010 с пакетом обновления 1 можно использовать, если у вас уже есть.

Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). В качестве альтернативы можно отдельно установить необходимое программное обеспечение, используя следующие ссылки:

- [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)

Если вы используете Visual Studio 2010 вместо Visual Web Developer, установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Этом руководстве предполагается, вы выполнили [начало работы с MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) руководства или что вы знакомы с разработкой приложений ASP.NET MVC. Это руководство начинается с завершенный проект из [начало работы с MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) руководства.

Этот учебник показывает код на языке C#. Тем не менее [начальный проект](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) завершенного проекта также доступны в Visual Basic.

Проект Visual Studio с исходным кодом C# и Visual Basic в этой статье прилагаются: [загрузить](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Что вы создадите

Вам предстоит добавить шаблоны (в частности, редактировать и отображать шаблоны) в простое приложение списка фильмов, который был создан в [начало работы с MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) руководства. Вы также добавите [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) всплывающего календаря выбора дат для упрощения процесса ввода даты. На следующем рисунке показан модифицированной программы с jQuery элемент интерфейса всплывающего календаря отображения.

![по завершении jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Вот, вы узнаете, как:

- Как использовать атрибуты из [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространство имен для управления форматом данных, когда она появится и когда он находится в режиме редактирования.
- Создание шаблонов (редактировать и отображать шаблоны) для управления форматированием данных.
- Как добавить [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) как способ введите полей дат.

### <a name="getting-started"></a>Начало работы

Если у вас еще нет приложения фильма из начального проекта, скачайте его с помощью следующей ссылки: [загрузить](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Щелкните в обозревателе Windows щелкните правой кнопкой мыши *MvcMovie.zip* файл и выберите **свойства**. В **свойства MvcMovie.zip** выберите **Unblock**. (Разблокировка устраняет предупреждение системы безопасности, возникающее при попытке использовать *ZIP-файл* файл, загруженный из Интернета.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Щелкните правой кнопкой мыши *MvcMovie.zip* файл и выберите **извлечь все** Чтобы распаковать файл. В Visual Studio 2010 или Visual Web Developer откройте *MvcMovieCS\_TU.sln* файла.

В **обозревателе решений**, дважды щелкните *Views\Shared\\_Layout.cshtml* чтобы открыть его. Изменение `H1` заголовок из **Киноприложение MVC** для **фильма jQuery**. Нажмите клавиши CTRL + F5, чтобы запустить приложение и нажмите кнопку **Главная** вкладки, которая приводит к переходу на `Index` метода контроллера movie. Чтобы проверить приложение, выберите **изменить** ссылку и **сведения** ссылку на один из них. Обратите внимание, что в индексе, Правка, и хорошо форматирования представления сведений, Дата выпуска и Цена:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Форматирование даты и цены — это результат использования [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут свойств `Movie` класса.

Откройте *Movie.cs* файл и закомментируйте `DisplayFormat` атрибут `ReleaseDate` и `Price` свойства. Полученный в результате `Movie` класс выглядит следующим образом:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Нажмите клавиши CTRL + F5 еще раз, чтобы запустить приложение и выберите **Главная** вкладку, чтобы просмотреть список фильмов. На этот раз Дата выпуска показывает дату и время, а поле цена больше не отображается символ валюты. Изменения в `Movie` класс отменила соответствующего форматирования, который вы уже видели, но мы исправим это чуть позже.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>С помощью атрибутов DataAnnotations DataType для указания типа данных

Замените закомментированных `DisplayFormat` для атрибута `ReleaseDate` свойство с [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибут, с помощью `Date` перечисления. Замените `DisplayFormat` для атрибута `Price` свойство с [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибут опять же, это время с помощью `Currency` перечисления. Вот как выглядит готовый код:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Запустите приложение. Теперь дата выпуска цена свойства форматирования и правильно (которые, используя соответствующие форматы даты и валюты). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибут предоставляет метаданные типа для встроенных ASP.NET MVC шаблоны, отображения полей в правильном формате. С помощью `DataType` атрибут предпочтительнее использования `DisplayFormat` атрибут, который был изначально в коде, так как `DataType` атрибут делает модель яснее и гибче целях, например интернационализации.

В следующем разделе вы увидите, как создать пользовательские шаблоны для отображения полей даты.

> [!div class="step-by-step"]
> [Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
