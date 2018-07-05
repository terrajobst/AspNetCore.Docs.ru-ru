---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Использование HTML5 и элемент интерфейса всплывающего календаря на jQuery в ASP.NET MVC. часть 4 | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник поможет основные сведения о работе с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 74bd88408c4d60032a1275aa9f8540cf710dac36
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380912"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Использование HTML5 и элемент интерфейса всплывающего календаря на jQuery в ASP.NET MVC. часть 4
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этом учебнике описываются основы работы с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в приложении ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Добавление шаблона для редактирования даты

В этом разделе вы создадите шаблон для изменения дат, которые будут применяться, когда ASP.NET MVC отображает пользовательский Интерфейс для редактирования свойств модели, которые помечены атрибутом **даты** перечисление [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибута. Шаблон будет отображаться только дата; время не отображается. В шаблоне используется [Datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) всплывающего календаря выбора дат для предоставления способа для изменения дат.

Для начала работы откройте *Movie.cs* файл и добавьте [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут с **даты** перечисления `ReleaseDate` свойства, как показано в следующем коде:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Этот код приводит к `ReleaseDate` поля для отображения без времени, как отобразить шаблоны и редактирование шаблонов. Если приложение содержит *date.cshtml* шаблона в *Views\Shared\EditorTemplates* папку или в *Views\Movies\EditorTemplates* папка, этот шаблон будет использоваться для подготовки к просмотру любые `DateTime` свойство во время редактирования. В противном случае встроенных системных шаблонов ASP.NET будет отображать свойство как дата.

Нажмите CTRL+F5, чтобы запустить приложение. Выберите ссылку изменения убедитесь, что в поле ввода для Дата выпуска отображается только дата.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

В **обозревателе решений**, разверните *представления* папку, разверните *Shared* папку затем щелкните правой кнопкой *Views\Shared\EditorTemplates* папки.

Нажмите кнопку **добавить**, а затем нажмите кнопку **представление**. **Добавление представления** диалоговое окно.

В **Имя_представления** введите &quot;даты&quot;.

Выберите **создать как частичное представление** "флажок". Убедитесь, что **макета или главная страница** и **создать строго типизированное представление** флажки не выбраны.

Нажмите кнопку **Добавить**. *Views\Shared\EditorTemplates\Date.cshtml* создания шаблона.

Добавьте следующий код, чтобы *Views\Shared\EditorTemplates\Date.cshtml* шаблона.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

В первой строке объявляется модель должна быть `DateTime` типа. Несмотря на то, что вам не нужно объявить тип модели, в меню Правка и отобразить шаблоны, это рекомендуется, чтобы получить во время компиляции проверки модели, передаваемые в представление. (Еще одно преимущество — затем получите IntelliSense для модели в представлении в Visual Studio.) Если тип модели не объявлен, ASP.NET MVC рассматривает его [динамическое](https://msdn.microsoft.com/library/dd264741.aspx) введите и имеется не во время компиляции проверку типов. Если вы объявляете модель должна быть `DateTime` типа, он становится строго типизированными.

Вторая строка — просто литерала разметки HTML, который отображает &quot;с использованием шаблона даты&quot; перед поле даты. Вы с помощью этой строки временно убедитесь, что используется этот шаблон даты.

Следующая строка представляет [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) вспомогательный, который выполняет визуализацию `input` поле, которое представляет собой текстовое поле. Третий параметр для вспомогательного метода используется анонимный тип на класс для текстового поля, чтобы установить `datefield` и тип `date`. (Поскольку `class` является зарезервированным в C#, необходимо использовать `@` символ для экранирования `class` атрибут в анализатор C#.)

`date` Типа является тип входных данных HTML5, позволяющий HTML5 с поддержкой браузеров для отображения элемента управления calendar HTML5. Позже вы добавите код JavaScript, чтобы подключить datepicker jQuery для `Html.TextBox` элемента с помощью `datefield` класса.

Нажмите CTRL+F5, чтобы запустить приложение. Можно убедиться, что `ReleaseDate` свойства в представлении редактирования использует изменить шаблон, поскольку шаблон отображается &quot;с использованием шаблона даты&quot; непосредственно перед `ReleaseDate` поле ввода текста, как показано на этом рисунке:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Просмотрите исходный код страницы в браузере. (Например, щелкните страницу правой кнопкой мыши и выберите **Просмотр исходного кода**.) Ниже примере показаны некоторые разметки страницы, иллюстрирующая `class` и `type` атрибуты в формате HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Вернитесь к *Views\Shared\EditorTemplates\Date.cshtml* шаблона и remove &quot;с использованием шаблона даты&quot; разметки. Теперь завершенного шаблона выглядит следующим образом:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Добавление jQuery элемент интерфейса всплывающего календаря с помощью NuGet

В этом разделе вы добавите [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) всплывающего календаря выбора дат в шаблоне редактирования даты. [Пользовательский Интерфейс jQuery](http://jqueryui.com/) библиотеки обеспечивает поддержку анимации, расширенные эффекты и настраиваемые мини-приложения. Он основан на библиотеку jQuery JavaScript. Элемент всплывающего календаря делает простым и естественным вводить даты в календаре вместо ввода строки. Всплывающего календаря выбора дат также ограничивает пользователям юридические даты — запись обычных текстовых даты позволит вам ввести `2/33/1999` (февраля 33rd, 1999), но [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) всплывающего календаря выбора дат не позволит выбрать.

Во-первых необходимо установить библиотеки пользовательского интерфейса jQuery. Чтобы сделать это, вы используете NuGet, который представляет собой диспетчер пакетов, включенный в версии с пакетом обновления 1 Visual Studio 2010 и Visual Web Developer.

В Visual Web Developer из **средства** меню, выберите **диспетчер пакетов библиотеки** , а затем выберите **управление пакетами NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Примечание: Если **средства** не отображает меню **диспетчер пакетов библиотеки** команды, необходимо установить пакет NuGet, следуя инструкциям [Установка NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) страницы на веб-сайте NuGet.   
  
Если вы используете Visual Studio вместо Visual Web Developer из **средства** меню, выберите **диспетчер пакетов библиотеки** , а затем выберите **добавьте ссылки на пакет библиотеки**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

В **MVCMovie - управление пакетами NuGet** диалоговом окне щелкните **Online** вкладке слева, а затем введите &quot;jQuery.UI&quot; в поле поиска. Выберите j **запросов пользовательского интерфейса мини-приложения: Datepicker**, а затем выберите **установить** кнопки.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet добавляет эти отладочные версии и минифицированные версии jQuery Core пользовательского интерфейса и средством выбора дат jQuery в проект:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jQuery.UI.DatePicker.min.js*

Примечание: Отладочные версии (файлы без *. min.js* расширение) полезны для отладки, но на рабочем узле, следует включить только минифицированные версии.

Чтобы фактически использовать средство выбора дат jQuery, необходимо создать сценарий jQuery, который будет подключить мини-приложение календаря для редактирования шаблона. В **обозревателе решений**, щелкните правой кнопкой мыши *сценарии* папку и выберите **добавить**, затем **новый элемент**, а затем **JScript Файл**. Назовите файл *DatePickerReady.js*.

Добавьте следующий код, чтобы *DatePickerReady.js* файла:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Если вы не знакомы с jQuery, здесь приводится краткое описание того, что это делает: первая строка представляет собой &quot;готовности jQuery&quot; функцию, которая вызывается, когда все элементы модели DOM на странице загрузки. Вторая строка выбирает все элементы DOM, которые содержат имя класса `datefield`, затем вызывает `datepicker` функции для каждого из них. (Помните, что вы добавили `datefield` класс *Views\Shared\EditorTemplates\Date.cshtml* шаблона ранее в этом руководстве.)

Затем откройте *Views\Shared\\_Layout.cshtml* файла. Вам потребуется добавить ссылки на следующие файлы, которые являются обязательными, таким образом, можно использовать средство выбора даты.

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/Themes/Base/jQuery.UI.Theme.CSS*
- *jquery.ui.core.min.js*
- *jQuery.UI.DatePicker.min.js*
- *DatePickerReady.js*

В следующем примере показано фактический код, что следует добавить в нижней части `head` элемент в *Views\Shared\\_Layout.cshtml* файла.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Полный `head` разделе показан здесь:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL-адрес содержимого вспомогательный](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) метод преобразует путь к ресурсу в абсолютный путь. Необходимо использовать `@URL.Content` правильно ссылаться на эти ресурсы при работе приложения в IIS.

Нажмите CTRL+F5, чтобы запустить приложение. Выберите ссылку изменения, а затем поместите точку вставки в **ReleaseDate** поля. Отображается jQuery интерфейса всплывающего календаря.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Как и большинство элементов управления jQuery datepicker позволяет значительно изменять. Сведения см. в разделе [Visual настройки: проектирование тему пользовательского интерфейса jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) на [пользовательский Интерфейс jQuery](http://learn.jquery.com/jquery-ui/getting-started/) сайта.

### <a name="supporting-the-html5-date-input-control"></a>Поддержка HTML5 даты элемент управления для ввода

Так как все больше браузеров поддерживают HTML5, необходимо использовать собственный HTML5, входные данные, такие как `date` входного элемента, а не использовать календарь пользовательского интерфейса jQuery. Можно добавить логику в приложение на автоматическое использование элементов управления HTML5, если браузер поддерживает их. Для этого замените содержимое файла *DatePickerReady.js* файл со следующими:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

В первой строке этого сценария используется Modernizr для проверки того, что поддерживается ввод даты в HTML5. Если не поддерживается средством выбора дат jQuery вместо подключен. ([Modernizr](http://www.modernizr.com/docs/) — это библиотека JavaScript открытым исходным кодом, которая определяет доступность собственных реализаций HTML5 и CSS3. Modernizr входит во все новые проекты ASP.NET MVC, создаваемые.)

После внесения этого изменения, его можно проверить с помощью браузера, который поддерживает HTML5, например Opera 11. Запустите приложение с помощью HTML5-совместимые браузера и измените запись фильма. Элемент управления HTML5 даты используется вместо jQuery интерфейса всплывающего календаря:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Поскольку новые версии браузеров постепенно реализации HTML5, хороший подход сейчас является добавление кода на веб-сайт, который допустим для самых разнообразных поддержкой HTML5. Например, более надежным *DatePickerReady.js* сценарий показан ниже, которая позволяет вашего сайта поддерживают браузеры, частично поддерживающие управления даты HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Этот скрипт выбирает HTML5 `input` элементов типа `date` , не полностью поддерживают элемент управления HTML5 даты. Для этих элементов, он подключает jQuery интерфейса всплывающего календаря и затем изменяет `type` из атрибута `date` для `text`. Изменив `type` из атрибута `date` для `text`, частичная поддержка HTML5 даты исключается. Повысьте надежность *DatePickerReady.js* можно найти в [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Добавление допускает значения NULL даты шаблоны

Если использовать один из существующих шаблонов даты и передать null даты, вы получите ошибку времени выполнения. Для повышения надежности даты шаблоны, вам предстоит изменить их для обработки значения null. Для поддержки дат, допускающий значение NULL, измените код в *Views\Shared\DisplayTemplates\DateTime.cshtml* следующим:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Код возвращает пустую строку, если модель находится **null**.

Измените код в *Views\Shared\EditorTemplates\Date.cshtml* файла следующее:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

При выполнении этого кода, если модель не равно null, модели `DateTime` используется значение. Если модель имеет значение null, вместо него используется текущая дата.

### <a name="wrapup"></a>— Сводка

Этот учебник Описывает основы шаблонизированные вспомогательные объекты ASP.NET и показано, как с помощью jQuery элемент интерфейса всплывающего календаря в приложении ASP.NET MVC. Дополнительные сведения попробуйте следующие ресурсы:

- Сведения о локализации, см. в разделе блога по Rajeesh [JQueryUI Datepicker в ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Сведения о пользовательский Интерфейс jQuery, см. в разделе [пользовательский Интерфейс jQuery](http://docs.jquery.com/UI).
- Сведения о локализации элемент управления datepicker, см. в разделе [пользовательского интерфейса/Datepicker/локализации](http://docs.jquery.com/UI/Datepicker/Localization).
- Дополнительные сведения о шаблонах ASP.NET MVC см. в статье серии публикаций в блоге Брэда уилсона: на [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Несмотря на то, что серии была написана для ASP.NET MVC 2, материал по-прежнему применяется для текущей версии ASP.NET MVC.

> [!div class="step-by-step"]
> [Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
