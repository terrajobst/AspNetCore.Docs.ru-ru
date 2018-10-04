---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Использование вспомогательного метода DropDownList в ASP.NET MVC | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 11b874d2d07c84631c6c5c266c22c6de49d40cf2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578072"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Использование вспомогательного метода DropDownList в ASP.NET MVC.
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

Этом учебнике вы узнаете основы работы с [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) вспомогательный и [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) вспомогательная функция в ASP.NET веб-приложение MVC. Можно использовать Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio для прохождения учебника. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:

- [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)

Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Этом руководстве предполагается, вы выполнили [введение в ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) руководства или[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) руководства или вы знакомы с разработкой ASP.NET MVC. Это руководство начинается с измененный проект из [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) руководства. Можно загрузить начальный проект с помощью следующей ссылки [загрузить версии C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Проект Visual Web Developer с полное руководство исходный код C# доступен на следующей странице в этом разделе. [Скачайте](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Что вы создадите

Вы создадите методы действий и представления, которые используют [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) вспомогательный для выбора категории. Вы также будете использовать **jQuery** добавить появится диалоговое окно категории insert, который может использоваться, когда требуется новую категорию (например, Жанр или исполнитель). Ниже приведен снимок экрана представления создания, показывающая связи, чтобы добавить новый жанр и добавить новый исполнитель.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Вот, вы узнаете, как:

- Как использовать [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) вспомогательный метод для выбора категории данных.
- Как добавить **jQuery** диалогового окна для добавления новых категорий.

### <a name="getting-started"></a>Начало работы

Начните с загрузки начального проекта с помощью следующей ссылке: [загрузить](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). В обозревателе Windows щелкните правой кнопкой мыши *DDL\_Starter.zip* файл и выберите пункт Свойства. В **DDL\_Starter.zip свойства** диалоговом окне выберите разблокировать.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Щелкните правой кнопкой мыши DDL\_Starter.zip файлу и выберите **извлечь все** Чтобы распаковать файл. Откройте *StartMusicStore.sln* файла с помощью Visual Web Developer 2010 Express («Visual Web Developer» или «VWD» для краткости) или Visual Studio 2010.

Нажмите клавиши CTRL + F5, чтобы запустить приложение и нажмите кнопку **теста** ссылку.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Выберите **выберите категории фильма (простое)** ссылку. Будет открыто списка выберите тип фильм, комедия выбранное значение.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Щелкните правой кнопкой мыши в браузере, выберите Просмотр исходного кода. Отображается HTML для страницы. В приведенном ниже коде показан HTML-код для элемента выбора.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Вы увидите, что каждый элемент в списке выбора имеет значение (0, для действия, 1 для "Драма" и 2 для комедия 3 для продаваемая) и отображаемое имя (действие, "Драма", комедия и научной фантастикой). Приведенный выше код представляет стандартный HTML для списка выбора.

Изменить список выбора для строки "Драма" и нажмите клавишу **отправить** кнопки. URL-адрес в браузере `http://localhost:2468/Home/CategoryChosen?MovieType=1` и на странице отображается **выбрано: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Откройте *Controllers\HomeController.cs* файл и просмотрите `SelectCategory` метод.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) требуется вспомогательный, используемый для создания HTML-списка выбора **IEnumerable&lt;SelectListItem &gt;** , явно или неявно. То есть вы можете передать **IEnumerable&lt;SelectListItem &gt;**  явно присвоено [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) вспомогательный или добавить **IEnumerable&lt; SelectListItem &gt;**  для [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) тем же именем, для **SelectListItem** как свойство модели. Передавая **SelectListItem** явно или неявно рассматривается в следующей части руководства. В приведенном выше коде показан простейший возможный способ создания **IEnumerable&lt;SelectListItem &gt;**  и заполнить его текст и значения. Примечание `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) имеет [выбранные](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) свойство значение **true;** в результате отрисовки списка выберите для отображения **комедия** как выбранный элемент в списке.

**IEnumerable&lt;SelectListItem &gt;**  созданные выше добавляется [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) с именем MovieType. Это определяет способ передачи **IEnumerable&lt;SelectListItem &gt;**  явно присвоено [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) вспомогательный, показано ниже.

Откройте *Views\Home\SelectCategory.cshtml* файл и проверьте разметку.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

В третьей строке, мы устанавливаем макет представления/Общие/\_простой\_Layout.cshtml, который является упрощенной версией файла стандартным макетом. Мы это сделать, чтобы сохранить соответствующие элементы и подготовки к просмотру HTML простой.

В этом примере мы не изменяется состояние приложения, поэтому мы, передает данные с помощью **HTTP GET**, а не **HTTP POST**. См. в разделе W3C [краткий контрольный список для выбора HTTP GET или POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Так как мы не изменения приложения и обратной передачи формы, мы используем [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) перегрузку, которая позволяет указать метод действия, контроллера и формы метод (**HTTP POST** или **HTTP GET**). Обычно содержат представлений [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) перегрузку, которая не принимает никаких параметров. Версия не параметр по умолчанию для отправки данных формы POST версию того же метода действия и контроллера.

В следующей строке

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

передает строковый аргумент для **DropDownList** вспомогательный. Эта строка «MovieType» в нашем примере делает две вещи:

- Он предоставляет ключ для **DropDownList** вспомогательный метод для поиска **IEnumerable&lt;SelectListItem &gt;**  в **ViewBag**.
- Он является привязкой к данным на форме элемент MovieType. Если метод отправки **HTTP GET**, `MovieType` будет строку запроса. Если метод отправки **HTTP POST**, `MovieType` будут добавляться в тело сообщения. На следующем рисунке показана строка запроса со значением 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

В следующем коде показан `CategoryChosen` метод, чтобы отправить форму.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Вернитесь к странице теста и выберите **HTML SelectList** ссылку. HTML-страницы представляет элемент select, подобно тому, простой тестовой страницы ASP.NET MVC. В окне браузера щелкните правой кнопкой мыши и выберите **Просмотр исходного кода**. HTML-разметка для списка выбора по сути идентичны. Тестовой HTML-страницы, который выступает в метода действия ASP.NET MVC и представление, которое мы протестировали ранее.

### <a name="improving-the-movie-select-list-with-enums"></a>Улучшение в список Select фильма с перечислений

Если категории в приложении являются фиксированными и не меняется, можно воспользоваться преимуществами перечислений, чтобы сделать код более надежным и проще расширить. Когда вы добавляете новую категорию, создается значение соответствующей категории. Позволяет избежать ошибок копирования и вставки, если добавить новую категорию, но необходимо обновить значение категории.

Откройте *Controllers\HomeController.cs* файл и просмотрите следующий код:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Перечисления](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` захватывает типы четыре фильма. `SetViewBagMovieType` Метод создает **IEnumerable&lt;SelectListItem &gt;**  из `eMovieCategories` **перечисления**и задает `Selected` свойства из `selectedMovie` параметра. `SelectCategoryEnum` Метод действия использует же представление, что `SelectCategory` метода действия.

Перейдите на страницу тестирования и щелкните `Select Movie Category (Enum)` ссылку. На этот раз вместо значения (число) отображается строка, представляющая перечисления отображается.

### <a name="posting-enum-values"></a>Публикуя значения перечислений

HTML-формах обычно используются для отправки данных на сервер. В следующем коде показан `HTTP GET` и `HTTP POST` версиях `SelectCategoryEnumPost` метод.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Путем передачи `eMovieCategories` перечисления `POST` метод, можно извлечь значение перечисления и строка перечисления. Запуск образца и перейдите на страницу тестирования. Щелкните `Select Movie Category(Enum Post)` ссылку. Выберите тип фильм, а затем нажмите кнопку «Отправить». На экране отобразится значение и имя типа фильма.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Создание нескольких выберите элемент раздела

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) вспомогательный метод HTML отображает HTML- `<select>` элемент с `multiple` атрибут, который позволяет пользователям выбрать несколько элементов. Перейдите по ссылке теста, а затем выберите **Выбор страны с несколькими** ссылку. Готовый для просмотра пользовательского интерфейса можно выбрать несколько стран. В приведенном ниже рисунке выбраны Канада и Китай.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Изучение кода MultiSelectCountry

Изучите следующий код из *Controllers\HomeController.cs* файла.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Метод создает список стран, а затем передает его `MultiSelectList` конструктор. `MultiSelectList` Перегрузку конструктора, используемый в `GetCountries` описанный выше метод принимает четыре параметра:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *элементы*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) содержащий элементы в списке. В приведенном выше примере, список стран.
2. *DataValueField используются*: имя свойства в **IEnumerable** список, содержащий значение. В примере выше `ID` свойство.
3. *dataTextField*: имя свойства в **IEnumerable** список, содержащий сведения для отображения. В примере выше `name` свойство.
4. *selectedValues*: список выбранных значений.

В примере выше `MultiSelectCountry` передает метод `null` значение для выбранных стран, чтобы не выделять не странах при отображении пользовательского интерфейса. В следующем коде показано разметку Razor, используемую для отрисовки `MultiSelectCountry` представления.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Вспомогательный метод HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) использовать метод выше принимают два параметра: имя свойства для привязки модели и [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) содержащий выберите параметры и значения. `ViewBag.YouSelected` Приведенный выше код используется для отображения значений из стран, выбранный при отправке формы. Изучите HTTP POST перегрузку `MultiSelectCountry` метод.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Динамических свойств содержит выбранных странах, полученный для `Countries` записи в виде коллекции. В этой версии метод GetCountries передает список выбранных странах, поэтому если `MultiSelectCountry` отображается представление, в выбранных странах выбраны в пользовательском Интерфейсе.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Что делает выберите элемент понятное с jQuery Harvest выбранного подключаемого модуля

Harvest [выбранного](http://harvesthq.github.com/chosen/) HTML можно добавить подключаемый модуль jQuery &lt;выберите&gt; элемент, чтобы создать пользователя понятного пользовательского интерфейса. Приведенные ниже изображения демонстрации Harvest [выбранного](http://harvesthq.github.com/chosen/) подключаемый модуль jQuery с помощью `MultiSelectCountry` представления.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

В приведенные ниже, два изображения **Канада** выбран.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

В приведенном выше рисунке выбрана Канады и на нем **x** можно щелкнуть, чтобы удалить выделение. На рисунке ниже показана Канаде, Китае, и выбран Японии.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Подключая jQuery Harvest выбранного подключаемого модуля

В следующем разделе представлен нагляднее, если у вас есть некоторый опыт работы с jQuery. Если вы ранее не использовали jQuery перед, можно выполнить одно из следующих учебников jQuery.

- [Как работает jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) по [John Resig](http://ejohn.org/)
- [Начало работы с jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) по [Jörn Zaefferer](http://bassistance.de/)
- [Live примеры jQuery](http://codylindley.com/blogstuff/js/jquery/#) по [Lindley Коди](http://codylindley.com/)

Выбранный подключаемый модуль включен в начальный и готовый пример проекты данного учебника. Для этого учебника будет достаточно позволяет подключить этот пользовательский Интерфейс jQuery. Чтобы использовать подключаемый модуль jQuery выбранного Harvest в проект ASP.NET MVC, необходимо следующее:

1. Загрузите подключаемый модуль выбран из [github](https://github.com/harvesthq/chosen/). Этот шаг завершения для вас.
2. Добавьте выбранные папки в проект ASP.NET MVC. Добавьте ресурсы из выбранного подключаемого модуля, Скачанный на предыдущем шаге, к выбранной папке. Этот шаг завершения для вас.
3. Подключить выбранный подключаемый модуль для **DropDownList** или **ListBox** вспомогательный метод HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Подключение выбранного подключаемого модуля к представлению MultiSelectCountry.

Откройте *Views\Home\MultiSelectCountry.cshtml* файл и добавьте `htmlAttributes` параметр `Html.ListBox`. Вы добавите параметр содержит имя класса для списка выбора (`@class = "chzn-select"`). Ниже приведен полный код:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

В приведенном выше коде мы добавим атрибут HTML и значение атрибута `class = "chzn-select"`. \@ Символ выше класс не имеет ничего общего с представлений Razor. `class` — [ключевое слово C#](https://msdn.microsoft.com/library/x53a06bb.aspx). Ключевые слова C# нельзя использовать в качестве идентификаторов, если они не включены \@ префиксом. В примере выше `@class` является допустимым идентификатором, но **класс** не, так как **класс** является ключевым словом.

Добавьте ссылки на *Chosen/chosen.jquery.js* и *Chosen/chosen.css* файлов. *Chosen/chosen.jquery.js* и реализует функционально выбранного подключаемого модуля. *Chosen/chosen.css* файл предоставляет стилей. Добавьте следующие ссылки в нижнюю часть *Views\Home\MultiSelectCountry.cshtml* файла. Ниже показано, как ссылаться на выбранный подключаемый модуль.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Активация выбранного подключаемого модуля, используя имя класса, используемые в **Html.ListBox** кода. В приведенном выше примере имя класса — `chzn-select`. Добавьте следующую строку в конец *Views\Home\MultiSelectCountry.cshtml* файла представления. Эта строка активирует выбранного подключаемого модуля.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Следующая строка — это синтаксис для вызова функции готовы jQuery, который выбирает элемент DOM с именем класса `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Упакованный набор, возвращаемый из приведенного выше вызова затем применяет выбранный метод (`.chosen();`), который подключает выбранного подключаемого модуля.

В следующем коде показано завершенное *Views\Home\MultiSelectCountry.cshtml* файла представления.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Запустите приложение и перейдите к `MultiSelectCountry` представления. Попробуйте добавить и удалить странах. Загрузить образец также содержит `MultiCountryVM` метод и представление, который реализует функциональные возможности MultiSelectCountry, с помощью представления модели вместо **ViewBag**.

В следующем разделе вы увидите, как работает механизм формирования шаблонов ASP.NET MVC с **DropDownList** вспомогательный.

> [!div class="step-by-step"]
> [Вперед](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
