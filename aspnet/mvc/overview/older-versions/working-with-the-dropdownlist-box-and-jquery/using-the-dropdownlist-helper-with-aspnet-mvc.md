---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: "Использование вспомогательного метода DropDownList в ASP.NET MVC | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 278d04aec68e93f3ebfd12d06a96b59f3bcbef4b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Использование вспомогательного метода DropDownList в ASP.NET MVC
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

Этот учебник поможет узнать основы работы с [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) вспомогательный и [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) вспомогательный в приложении MVC ASP.NET. Можно использовать Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio, чтобы следовать учебнику. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:

- [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)

Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). В этом учебнике предполагается завершения [введение в ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) учебник или[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) учебника, или вы знакомы с разработкой ASP.NET MVC. Этот учебник начинается с измененный проект из [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) учебника. Вы можете загрузить начальный проект с помощью следующей ссылки [загрузки версии C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Проект Visual Web Developer в завершенной учебник исходный код C# доступна по следующему адресу. [Загрузить](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Что мы создадим

Вы создадите методы действий и представления, которые используют [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) поддержки, чтобы выбрать категорию. Вы также будете использовать **jQuery** для добавления, можно использовать при необходимости новой категории (например, Жанр или исполнителя) диалогового окна категории insert. Ниже приведен снимок экрана представления создания, показывающая связи, чтобы добавить новый жанр и добавить новый исполнитель.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Ниже приведен изучаемого материала.

- Как использовать [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) вспомогательный метод для выбора категории данных.
- Добавление **jQuery** диалоговое окно, чтобы добавить новые категории.

### <a name="getting-started"></a>Начало работы

Загрузить через Интернет начальный проект с по следующей ссылке: [загрузить](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). В проводнике Windows щелкните правой кнопкой мыши *DDL\_Starter.zip* файл и выберите пункт Свойства. В **DDL\_Starter.zip свойства** диалоговое окно, выберите разблокировать.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Щелкните правой кнопкой мыши DDL\_Starter.zip файлу и выберите **извлечь все** для распаковки файла. Откройте *StartMusicStore.sln* файл с Visual Web Developer 2010 Express («Visual Web Developer» или «VWD» для краткости) или Visual Studio 2010.

Нажмите клавиши CTRL + F5, чтобы запустить приложение и нажмите кнопку **тест** ссылку.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Выберите **выберите категории фильма (простое)** ссылку. Отобразится список фильмов выберите тип, содержащий комедия выбранное значение.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Щелкните правой кнопкой мыши в браузере и выберите Просмотр исходного кода. Отображается HTML для страницы. В следующем примере кода показано HTML для элемента select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Вы увидите, что каждый элемент в списке выбора имеет значение (0 для действия, 1 для Драма, комедия 2 и 3 для научной фантастики) и отображаемое имя (действие, Драма, комедия и научной фантастики). Приведенный выше код является стандартный код HTML для списка выбора.

Изменить список выбора для Драма и нажмите клавишу **отправить** кнопки. URL-адрес в браузере `http://localhost:2468/Home/CategoryChosen?MovieType=1` и на странице отображается **выбранных: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Откройте *Controllers\HomeController.cs* файл и проверьте `SelectCategory` метод.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) требует поддержки, используемый для создания HTML-списка выбора **IEnumerable&lt;SelectListItem &gt;** , явно или неявно. То есть, можно передать **IEnumerable&lt;SelectListItem &gt;**  явного [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) вспомогательный или добавить **IEnumerable&lt; SelectListItem &gt;**  для [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) с тем же именем, для **SelectListItem** как в свойстве модели. Передавая **SelectListItem** явно или неявно, описанные в следующей части этого руководства. Приведенный выше код показан простой возможных способов создания **IEnumerable&lt;SelectListItem &gt;**  и заполнить ее текст и значения. Примечание `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) имеет [выбранные](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) свойство **true;** это вызовет отображаемый список выбора для отображения **комедия** с выбранным элементом в списке.

**IEnumerable&lt;SelectListItem &gt;**  создан выше добавляется [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) с именем MovieType. Это, как мы передаем **IEnumerable&lt;SelectListItem &gt;**  явно присвоено [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) вспомогательный, показано ниже.

Откройте *Views\Home\SelectCategory.cshtml* файл и проверьте разметку.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

В третьей строке задается макета для представления/Общие/\_простой\_Layout.cshtml, который представляет собой упрощенную версию файла стандартный макет. Мы это сделать, чтобы сохранить отображение и подготовки к просмотру HTML простой.

В этом примере мы не изменялось состояние приложения, поэтому мы будет отправлять данные с помощью **HTTP GET**, а не **HTTP POST**. См. в разделе W3C [краткий контрольный список для выбора HTTP GET или POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Так как мы не изменения приложения и учета формы, мы используем [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) перегрузку, которая позволяет указать метод действия, контроллера и формы метод (**HTTP POST** или **HTTP GET**). Обычно представления содержат [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) перегрузку, которая не принимает никаких параметров. По умолчанию используется версия не параметр для отправки данных формы POST версии того же метода действия и контроллера.

В следующей строке

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

передает строковый аргумент для **DropDownList** вспомогательный. Эта строка «MovieType» в нашем примере выполняет следующие операции:

- Он предоставляет ключ для **DropDownList** вспомогательный метод для поиска **IEnumerable&lt;SelectListItem &gt;**  в **ViewBag**.
- Он является привязкой к данным на форму элемент MovieType. Если метод отправки **HTTP GET**, `MovieType` будет строка запроса. Если метод отправки **HTTP POST**, `MovieType` будет добавляться в тело сообщения. На следующем рисунке строку запроса со значением 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

В следующем коде показано `CategoryChosen` метод отправить форму.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Перейдите на страницу теста и выберите **HTML SelectList** ссылку. HTML-страницы представляет элемент select, подобно тому простой тестовой страницы ASP.NET MVC. Щелкните правой кнопкой мыши окно браузера и выберите **Просмотр исходного кода**. HTML-разметка для списка выберите сущности, отличаются. Тестовой HTML-страницы, он работает как система метода действия ASP.NET MVC и представление, которое мы протестировали ранее.

### <a name="improving-the-movie-select-list-with-enums"></a>Улучшение в список Select фильма с перечислений

Если категории в приложении фиксированы и не изменится, можно воспользоваться преимуществами перечислений, чтобы сделать код более надежным и простым для расширения. При добавлении новой категории, вычисляется значение подходящую категорию. Позволяет избежать ошибок копирования и вставки, если добавить новую категорию, но необходимо обновить значение категории.

Откройте *Controllers\HomeController.cs* файла и изучите следующий код:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Перечисления](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` захватывает типы четыре фильма. `SetViewBagMovieType` Метод создает **IEnumerable&lt;SelectListItem &gt;**  из `eMovieCategories` **перечисления**и задает `Selected` свойства из `selectedMovie` параметра. `SelectCategoryEnum` Метод действия использует одинаковое представление как `SelectCategory` метода действия.

Перейдите на страницу теста и выберите команду `Select Movie Category (Enum)` ссылку. На этот раз вместо значения (число) будет отображаться, строка, представляющая перечисления отображается.

### <a name="posting-enum-values"></a>Учет значения перечисления

HTML-формы, обычно используются для отправки данных на сервер. В следующем коде показано `HTTP GET` и `HTTP POST` версии `SelectCategoryEnumPost` метод.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Путем передачи `eMovieCategories` перечисление `POST` метод, мы можно извлечь значение enum и перечисления строки. Запуск образца и перейдите к странице теста. Щелкните `Select Movie Category(Enum Post)` ссылку. Выберите тип фильм, а затем нажмите кнопку "Отправить". На экране отобразится значение и имя типа фильма.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Создание нескольких выберите элемент раздела

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) вспомогательный метод HTML отображает HTML- `<select>` элемент с `multiple` атрибут, который позволяет пользователям выбрать несколько элементов. Перейдите по ссылке теста, а затем выберите **несколькими Выбор страны** ссылку. Готовый для просмотра пользовательского интерфейса можно выбрать несколько стран. На приведенном ниже рисунке выбраны Канады и Китая.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Анализ кода MultiSelectCountry

Изучите следующий код из *Controllers\HomeController.cs* файла.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Метод создает список стран, а затем передает его `MultiSelectList` конструктор. `MultiSelectList` Перегрузка конструктора, используемая в `GetCountries` описанный выше метод принимает четыре параметра:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *элементы*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) содержащие элементы в списке. В приведенном выше примере, список стран.
2. *DataValueField используются*: имя свойства в **IEnumerable** список, содержащий значение. В приведенном выше примере `ID` свойство.
3. *dataTextField*: имя свойства в **IEnumerable** список, который содержит сведения для отображения. В приведенном выше примере `name` свойство.
4. *selectedValues*: список выбранных значений.

В приведенном выше примере `MultiSelectCountry` метод передает `null` значение для выбранных стран, чтобы не выделять не странах, когда отображается пользовательский Интерфейс. В следующем коде показано Razor разметку, используемую для отрисовки `MultiSelectCountry` представления.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Вспомогательный метод HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) метод использованном выше принимают два параметра: имя свойства для привязки модели и [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) содержащая выберите параметры и значения. `ViewBag.YouSelected` Приведенный выше код используется для отображения значений из этих стран, выбранный при отправке формы. Изучите перегруженный HTTP POST `MultiSelectCountry` метод.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Динамических свойств содержит выбранной страны, полученный для `Countries` записи в виде коллекции. В этой версии GetCountries методу передается список выбранных стран, поэтому после `MultiSelectCountry` отображается представление, выбранным странам выбраны в пользовательском Интерфейсе.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Делая выбор элемента понятное с jQuery уязвимость выбранного подключаемого модуля

Уязвимость [выбранного](http://harvesthq.github.com/chosen/) jQuery подключаемого модуля могут добавляться в HTML- &lt;выберите&gt; элемент, чтобы создать пользователя понятное пользовательского интерфейса. Изображения ниже демонстрируют уязвимость [выбранного](http://harvesthq.github.com/chosen/) подключаемый модуль jQuery с `MultiSelectCountry` представления.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

В ниже, два изображения **Канада** выбран.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

В приведенном выше рисунке выбран Канады и на нем **x** можно щелкнуть, чтобы снять выделение. На рисунке ниже показана Канада, Китай, и выбран на японском языке.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Подключение jQuery уязвимость выбранного подключаемого модуля

Следующий раздел проще выполнить, если у вас есть опыт работы с jQuery. Если вы никогда не работали jQuery до, может потребоваться выполните одно из следующих учебников jQuery.

- [Как работает jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) по [Джон Resig](http://ejohn.org/)
- [Приступая к работе с jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) по [Jörn Zaefferer](http://bassistance.de/)
- [Примеры jQuery Live](http://codylindley.com/blogstuff/js/jquery/#) по [Cody Lindley](http://codylindley.com/)

Выбранный подключаемый модуль включен в начальный и завершенных образцов проектов, приведенных в этом учебнике. В этом учебнике будет достаточно позволяет подключить этот пользовательский Интерфейс jQuery. Чтобы использовать подключаемый модуль выбирается уязвимость jQuery в проект ASP.NET MVC, необходимо выполнить следующее:

1. Загрузите подключаемый модуль выбирается из [github](https://github.com/harvesthq/chosen/). Этот шаг выполнено для вас.
2. Добавьте выбранные папки в проект ASP.NET MVC. Добавьте ресурсы из выбранного подключаемого модуля, который был загружен на предыдущем шаге, в зависимости от выбранного папку. Этот шаг выполнено для вас.
3. Подключить выбранный подключаемый модуль для **DropDownList** или **ListBox** вспомогательный метод HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Подключение выбранного подключаемого модуля в MultiSelectCountry представление.

Откройте *Views\Home\MultiSelectCountry.cshtml* и добавьте `htmlAttributes` параметр `Html.ListBox`. Будет добавлен параметр содержит имя класса в списке select (`@class = "chzn-select"`). Ниже приведен полный код:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

В приведенном выше коде мы добавляем атрибут HTML и значение атрибута `class = "chzn-select"`. Символ @ перед класса никак не связано с представлений Razor. `class`— [ключевого слова C#](https://msdn.microsoft.com/library/x53a06bb.aspx). Ключевые слова C# не может использоваться как идентификаторы, если они содержат префикс. В приведенном выше примере `@class` является допустимым идентификатором, но **класса** не, так как **класс** является ключевым словом.

Добавьте ссылки на *Chosen/chosen.jquery.js* и *Chosen/chosen.css* файлов. *Chosen/chosen.jquery.js* и реализует функционально из выбранного подключаемого модуля. *Chosen/chosen.css* файл предоставляет стили. Добавьте эти ссылки в нижней части *Views\Home\MultiSelectCountry.cshtml* файла. Следующий код показано, как ссылаться на выбранный подключаемый модуль.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Активировать выбранный подключаемый модуль, используя имя класса, используемое в **Html.ListBox** кода. В предыдущем примере имя класса — `chzn-select`. Добавьте следующую строку в конец *Views\Home\MultiSelectCountry.cshtml* файл представления. Эта строка активирует выбранного подключаемого модуля.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Следующий синтаксис для вызова функции готовы jQuery, который выбирает элемент DOM с именем класса, является `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Упакованное затем набор, возвращаемый выше вызов применяется выбранного метода (`.chosen();`), который подключает выбранного подключаемого модуля.

В следующем коде показан завершенный *Views\Home\MultiSelectCountry.cshtml* файл представления.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Запустите приложение и перейдите к `MultiSelectCountry` представления. Повторите Добавление и удаление страны. Загружаемый образец предоставлен также содержит `MultiCountryVM` метод и представления, который реализует функциональность MultiSelectCountry, с помощью представления модели вместо **ViewBag**.

В следующем разделе вы увидите, как работает механизм формирование шаблонов ASP.NET MVC с **DropDownList** вспомогательный.

>[!div class="step-by-step"]
[Вперед](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
