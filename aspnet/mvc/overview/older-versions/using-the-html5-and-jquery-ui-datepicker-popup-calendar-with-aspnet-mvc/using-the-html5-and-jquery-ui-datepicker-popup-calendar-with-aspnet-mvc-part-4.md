---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 4 | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: c6df727107b0a045341badefbf99eec773cd4eff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874789"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 4
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в приложении MVC ASP.NET.


### <a name="adding-a-template-for-editing-dates"></a>Добавление шаблона для редактирования дат

В этом разделе вы создадите шаблон для изменения дат, которые будут применяться, когда ASP.NET MVC отображает пользовательский Интерфейс для редактирования свойств модели, которые отмечены **даты** перечисление [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибута. Шаблон будет отображаться только дата; время не отображается. В шаблоне используется [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) раскрывающегося календаря предоставляет способ для изменения дат.

Чтобы начать, откройте *Movie.cs* и добавьте [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибутом **даты** перечисления `ReleaseDate` свойства, как показано в следующем коде:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Этот код вызывает `ReleaseDate` поле для отображения без времени, как отобразить шаблоны и редактирование шаблонов. Если приложение содержит *date.cshtml* шаблона в *Views\Shared\EditorTemplates* папке или в *Views\Movies\EditorTemplates* папки, этот шаблон будет использоваться для отображения любого `DateTime` свойства во время редактирования. В противном случае встроенную систему шаблонов ASP.NET будет отображать свойство как дата.

Нажмите CTRL+F5, чтобы запустить приложение. Выберите ссылку правки, чтобы убедиться, что в поле ввода для даты выпуска отображается только дата.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

В **обозревателе решений**, разверните *представления* папку, разверните *Shared* папку, а затем щелкните правой кнопкой мыши *Views\Shared\EditorTemplates* папки.

Нажмите кнопку **добавить**, а затем нажмите кнопку **представление**. **Добавить представление** диалоговое окно.

В **имя представления** введите &quot;даты&quot;.

Выберите **создать как частичное представление** флажок. Убедитесь, что **макета или главная страница** и **создать строго типизированное представление** не установлены флажки.

Нажмите кнопку **Добавить**. *Views\Shared\EditorTemplates\Date.cshtml* создания шаблона.

Добавьте следующий код в *Views\Shared\EditorTemplates\Date.cshtml* шаблона.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

Первая строка объявляет модели, чтобы быть `DateTime` типа. Несмотря на то, что не требуется объявить тип модели в режиме изменения и отобразить шаблоны, он все же рекомендуется, чтобы получить во время компиляции проверки модели, передаваемые в представление. (Еще одним преимуществом является затем получить доступ к IntelliSense для модели в представлении в Visual Studio.) Если не был объявлен тип модели, ASP.NET MVC рассматривает его [динамическое](https://msdn.microsoft.com/library/dd264741.aspx) введите и времени компиляции не проверку типов. При объявлении модели, чтобы быть `DateTime` тип, он становится со строго типизированными.

Вторая строка — только текстовое разметки HTML, который отображает &quot;с использованием шаблона даты&quot; перед поле даты. Эта строка будет использоваться временно для убедитесь, что используется этот шаблон даты.

Следующая строка представляет [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) поддержки, который выполняет визуализацию `input` поле, которое представляет собой текстовое поле. Третий параметр для вспомогательного метода использует анонимный тип для класс для текстового поля, чтобы установить `datefield` и тип `date`. (Поскольку `class` является зарезервированным в языке C# необходимо использовать `@` символ для экранирования `class` атрибута в анализатор C#.)

`date` Тип является типом ввода HTML5, позволяет браузерам поддержкой HTML5 для отображения элемента управления calendar HTML5. Позднее вы добавите некоторые JavaScript, чтобы подключить datepicker jQuery для `Html.TextBox` элемента с помощью `datefield` класса.

Нажмите CTRL+F5, чтобы запустить приложение. Можно убедиться, что `ReleaseDate` свойства в представлении редактирования используется изменить шаблон, поскольку отображает шаблон &quot;с использованием шаблона даты&quot; непосредственно перед `ReleaseDate` текстовое поле, ввода, как показано на этом рисунке:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Просмотрите исходный код страницы в браузере. (Например, щелкните правой кнопкой мыши страницу и выберите **источник**.) Ниже примере показаны некоторые разметка страницы, демонстрирующая `class` и `type` атрибуты в HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Вернитесь к *Views\Shared\EditorTemplates\Date.cshtml* шаблона и удаление &quot;с использованием шаблона даты&quot; разметки. Теперь завершенного шаблона выглядит следующим образом:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Добавление раскрывающегося календаря с выбором дат jQuery с помощью NuGet

В этом разделе вы добавите [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) раскрывающегося календаря в шаблон Дата изменения. [JQuery UI](http://jqueryui.com/) библиотека поддерживает дополнительные настраиваемые мини-приложений и эффектов анимации. Она построена на основе библиотеки jQuery JavaScript. Раскрывающегося календаря с выбором дат позволяет легко и естественным ввода даты с помощью календаря вместо ввода строки. Раскрывающегося календаря также ограничивает пользователей в юридических даты — запись обычный текст для даты, которые позволяют введите примерно `2/33/1999` (февраль 33rd, 1999 г.), но [datepicker пользовательского интерфейса jQuery](http://jqueryui.com/demos/datepicker/) раскрывающегося календаря, не позволит.

Во-первых необходимо установить библиотеки пользовательского интерфейса jQuery. Для этого потребуется использовать NuGet, представляющего диспетчера пакетов, которое включено в версии с пакетом обновления 1 Visual Studio 2010 и Visual Web Developer.

В Visual Web Developer из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки** , а затем выберите **управление пакетами NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Примечание: Если **средства** не отображается меню **диспетчер пакетов библиотеки** команды, необходимо установить NuGet, следуя указаниям [Установка NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) страницы веб-сайте NuGet.   
  
Если вы используете Visual Studio, а не Visual Web Developer из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки** , а затем выберите **добавьте ссылку на пакет библиотеки**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

В **MVCMovie — управление пакетами NuGet** диалоговое окно, нажмите кнопку **Online** слева, а затем введите &quot;jQuery.UI&quot; в поле поиска. Выберите j **запроса пользовательского интерфейса мини-приложения: Datepicker**, а затем выберите **установить** кнопки.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet добавляет эти отладки и версии уменьшенный jQuery основных компонентов пользовательского интерфейса и выбора дат jQuery в проект:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Примечание: Отладочные версии (файлы без *. min.js* расширение) полезны для отладки, но в рабочего веб-сайта, следует включить только уменьшенный версии.

Действительно использовать средство выбора дат jQuery, необходимо создать сценарий jQuery, будет подключать мини-приложение календаря для редактирования шаблона. В **обозревателе решений**, щелкните правой кнопкой мыши *сценариев* папку и выберите **добавить**, затем **новый элемент**, а затем **JScript Файл**. Назовите файл *DatePickerReady.js*.

Добавьте следующий код в *DatePickerReady.js* файла:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Если вы не знакомы с jQuery, Вот краткое объяснение при этом произойдет: первая строка &quot;jQuery готов&quot; функции, которая вызывается, когда все элементы модели DOM на странице загрузки. Вторая строка выбирает все элементы модели DOM, с именем класса `datefield`, затем вызывает `datepicker` функции для каждого из них. (Помните, что вы добавили `datefield` класса *Views\Shared\EditorTemplates\Date.cshtml* шаблона ранее в этом учебнике.)

Затем откройте *представления\общие\\_Layout.cshtml* файла. Необходимо добавить ссылки на следующие файлы, которые необходимы, чтобы можно было использовать Выбор даты:

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

Следующий пример показывает фактический код, следует добавить в нижней части `head` элемент в *представления\общие\\_Layout.cshtml* файла.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Полный `head` раздела приведен ниже:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL-адрес содержимого вспомогательный](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) метод преобразует путь к ресурсу по абсолютному пути. Необходимо использовать `@URL.Content` правильно ссылаться на эти ресурсы при запуске приложения на IIS.

Нажмите CTRL+F5, чтобы запустить приложение. Выберите ссылки редактирования, а затем поместить курсор в **ReleaseDate** поля. Отображается всплывающим календарем jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Как и большинство элементов управления jQuery datepicker позволяет значительно изменять. Сведения см. в разделе [настройке Visual: проектирование пользовательского интерфейса jQuery темы](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) на [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) сайта.

### <a name="supporting-the-html5-date-input-control"></a>Поддержка HTML5 элемента управления вводом даты

Как дополнительные обозреватели поддерживают HTML5, может потребоваться использовать собственный HTML5, ввода, такие как `date` элементе ввода, а не использовать календарь пользовательского интерфейса jQuery. Можно добавить логику в приложение на автоматическое применение элементов управления HTML5, если браузер поддерживает их. Для этого замените содержимое *DatePickerReady.js* файл со следующим:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

В первой строке этого сценария используется Modernizr, чтобы убедиться, что поддерживается ввода даты в HTML5. Если не поддерживается, выбора дат jQuery вместо подключен. ([Modernizr](http://www.modernizr.com/docs/) — это библиотека JavaScript открытым исходным кодом, который определяет доступность собственные реализации HTML5 и CSS3. Modernizr включен в любые создаваемые проекты ASP.NET MVC.)

После внесения этого изменения, можно проверить его с помощью браузера, поддерживающего HTML5, такие как Opera 11. Запуск приложения с помощью браузера HTML5-совместимые и измените запись фильма. Вместо раскрывающегося календаря пользовательского интерфейса jQuery используется элемент управления HTML5 даты:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Поскольку новые версии браузеров постепенно реализации HTML5, теперь рекомендуется добавление кода в веб-сайт, который допустим для разнообразных поддержкой HTML5. Например, более надежным *DatePickerReady.js* сценарий показан ниже, которая позволяет вашего сайта поддержки браузеров, которые частично поддерживает элемент управления HTML5 даты.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Этот скрипт выбирает HTML5 `input` элементов типа `date` , не полностью поддерживают элемент управления HTML5 даты. Для этих элементов подключает всплывающим календарем jQuery и затем изменяет `type` атрибута из `date` для `text`. Изменив `type` атрибута из `date` для `text`, частичная поддержка даты HTML5 исключается. Еще более надежные *DatePickerReady.js* можно найти в [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Добавление шаблонов дат, допускающие значение NULL

Если использовать один из существующих шаблонов даты и передать значение null, дата, вы получите ошибку во время выполнения. Для повышения надежности шаблоны даты, вы измените их для обработки значения null. Для поддержки дат, допускающие значение NULL, измените код в *Views\Shared\DisplayTemplates\DateTime.cshtml* следующее:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Код возвращает пустую строку при использовании модели **null**.

Измените код в *Views\Shared\EditorTemplates\Date.cshtml* файла следующее:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Если этот код выполняется, если модель не равно null, модели `DateTime` используется значение. Если модель имеет значение null, вместо него используется текущая дата.

### <a name="wrapup"></a>— Сводка

Этот учебник описаны основы шаблонизированные вспомогательные объекты ASP.NET, а также демонстрируется использование раскрывающегося календаря с выбором дат jQuery UI в приложении ASP.NET MVC. Дополнительные сведения попробуйте следующие ресурсы:

- Сведения о локализации, см. в Rajeesh записи [JQueryUI Datepicker в ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Сведения о jQuery UI см. в разделе [jQuery UI](http://docs.jquery.com/UI).
- Сведения о локализации элемент управления datepicker см. в разделе [пользовательского интерфейса, Datepicker/локализации](http://docs.jquery.com/UI/Datepicker/Localization).
- Дополнительные сведения о шаблонах ASP.NET MVC см. в разделе серии публикаций в блоге Брэда Уилсона на [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Несмотря на то, что ряд была написана для ASP.NET MVC 2, материала по-прежнему применяется для текущей версии ASP.NET MVC.

> [!div class="step-by-step"]
> [Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
