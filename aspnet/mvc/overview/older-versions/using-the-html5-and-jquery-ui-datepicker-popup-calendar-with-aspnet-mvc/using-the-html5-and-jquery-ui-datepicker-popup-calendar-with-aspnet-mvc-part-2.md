---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Использование HTML5 и элемент интерфейса всплывающего календаря jQuery в ASP.NET MVC. часть 2 | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник поможет основные сведения о работе с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 572185507016afc26fc2e06ede55361b2daed277
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835693"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Использование HTML5 и элемент интерфейса всплывающего календаря jQuery в ASP.NET MVC. часть 2
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этом учебнике описываются основы работы с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в приложении ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Добавление шаблона автоматического даты и времени

В первой части этого руководства вы увидели, как можно добавлять атрибуты в модель, чтобы явно задать формат, и как можно явно указать шаблон, который используется для отображения модели. Например [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут в следующем коде явным задает форматирование для `ReleaseDate` свойство.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

В следующем примере [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибут, с помощью `Date` перечисление, указывает, что шаблон даты следует использовать для отображения модели. Если в проекте отсутствует шаблон даты, используется шаблон даты.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Тем не менее ASP. MVC можно выполнить сопоставление типов с помощью соглашения относительно конфигурации, ищет шаблон, который совпадает с именем типа. Это позволяет создать шаблон, который автоматически форматирует данные без использования атрибуты или код вообще. Для этой части руководства вы создадите шаблон, который автоматически применяется к свойствам модели типа [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Не используйте атрибут или другие конфигурации, чтобы указать, что шаблон должен использоваться для отображения всех свойств модели типа [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Вы также узнаете способ настроить внешний вид отдельных свойств или даже отдельных полей.

Для начала давайте удалить существующие сведения о форматировании и Отображение полного дат в приложении.

Откройте *Movie.cs* файл и закомментируйте `DataType` атрибут `ReleaseDate` свойства:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Нажмите CTRL+F5, чтобы запустить приложение.

Обратите внимание, что `ReleaseDate` свойство теперь отображает дату и время, потому что это значение по умолчанию, если нет сведения о форматировании.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Добавление стилей CSS для тестирования новых шаблонов

Перед созданием шаблона для форматирования дат, вы добавите несколько правил стилей CSS, которые можно применять в новые шаблоны. Это поможет вам убедиться, что отображаемой страницы использует новый шаблон.

Откройте *Content\Site.cs*s и добавьте следующие правила CSS для нижней части файла:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Добавление шаблонов отображения даты и времени

Теперь вы можете создать новый шаблон. В *Views\Movies* папке создайте *DisplayTemplates* папки.

В *Views\Shared* папке создайте *DisplayTemplates* папки и *EditorTemplates* папки.

Шаблоны отображения в *Views\Shared\DisplayTemplates* папки будут использоваться всеми контроллерами. Шаблоны отображения в *Views\Movie\DisplayTemplates* папки будут использоваться только `Movie` контроллера. (Если шаблон с таким именем появляется в обеих папках шаблона в *Views\Movie\DisplayTemplates* папки — то есть более конкретного шаблона — имеет более высокий приоритет для представлений, возвращенных `Movie` контроллер.)

В **обозревателе решений**, разверните *представления* папку, разверните *Shared* папку затем щелкните правой кнопкой *Views\Shared\DisplayTemplates* папки.

Нажмите кнопку **добавить** и нажмите кнопку **представление**. **Добавление представления** диалоговое окно.

В **Имя_представления** введите `DateTime`. (Это имя необходимо использовать для поиска соответствия имя типа.)

Выберите **создать как частичное представление** "флажок". Убедитесь, что **макета или главная страница** и **создать строго типизированное представление** флажки не выбраны.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Нажмите кнопку **Добавить**. Объект *DateTime.cshtml* шаблон создается в *Views\Shared\DisplayTemplates*.

На следующем рисунке показана *представления* папку в **обозревателе решений** после `DateTime` создаются шаблоны отображения и редактор.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Откройте *Views\Shared\DisplayTemplates\DateTime.cshtml* файл и добавьте следующую разметку, которая использует [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) метод для форматирования свойства как дата без времени. ( `{0:d}` Формат задает краткий формат даты.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Повторите этот шаг, чтобы создать `DateTime` шаблона в *Views\Movie\DisplayTemplates* папки. Используйте указанный ниже код в *Views\Movie\DisplayTemplates\DateTime.cshtml* файла.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` Класс CSS вызывает отображение даты, который будет отображаться красным полужирным шрифтом. Вы добавили `loud-1` класс CSS так же, как временной меры, чтобы можно было легко увидеть, когда используется данный шаблон.

Что вы делали создается и настраиваемые шаблоны, которые будут использовать ASP.NET для отображения дат. Более общие шаблона (в *Views\Shared\DisplayTemplates* папку) отображает простой короткого формата даты. Шаблон, который предназначен для `Movie` контроллера (в *Views\Movies\DisplayTemplates* папку) отображает краткий, также форматируется в виде красным полужирным шрифтом.

Нажмите CTRL+F5, чтобы запустить приложение. Браузер отображает представление Index для приложения.

`ReleaseDate` Теперь отображаются полужирным красным шрифтом без времени. Это поможет вам убедиться, что `DateTime` Шаблонизированный вспомогательный объект в *Views\Movies\DisplayTemplates* папка выбрана через `DateTime` Шаблонизированный вспомогательный объект, в общей папке (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Теперь Переименуйте *Views\Movies\DisplayTemplates\DateTime.cshtml* файл *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Нажмите CTRL+F5, чтобы запустить приложение.

Это время `ReleaseDate` свойство отображает дату без времени и без полужирным красным шрифтом. Это показывает, что тип шаблона, который имеет имя данных (в данном случае `DateTime`) автоматически используется для отображения всех свойств модели этого типа. После того как вы переименован *DateTime.cshtml* файл *LoudDateTime.cshtml*, ASP.NET не удается найти шаблон в *Views\Movies\DisplayTemplates* папки, чтобы его использовать *DateTime.cshtml* шаблон из * Views\Movies\Shared\* папки.

(Сопоставления шаблона без учета регистра, поэтому возможно, были созданы имя файла шаблона в любом регистре. Например *DATETIME.chstml, datetime.cshtml*, и *DaTeTiMe.cshtml* будет соответствовать всем `DateTime` тип.)

Для просмотра: на этом этапе `ReleaseDate` поле отображается с помощью *Views\Movies\DisplayTemplates\DateTime.cshtml* шаблон, который отображает данные, в формате короткой даты, но в противном случае добавляет нет особого формата.

### <a name="using-uihint-to-specify-a-display-template"></a>Использование UIHint для указания шаблона отображения

Если веб-приложения имеется много `DateTime` поля и по умолчанию, будет отображаться в формате только даты, все или большинство из них *DateTime.cshtml* шаблона — Хороший подход. Но что делать, если у вас есть несколько дат место для отображения полной даты и времени? Без проблем. Можно создать дополнительный шаблон и использовать [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут, чтобы задать формат полной даты и времени. Затем можно выборочно применить этот шаблон. Можно использовать [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут на уровне модели. также можно указать шаблон внутри представления. В этом разделе вы узнаете, как использовать `UIHint` атрибут, чтобы выборочно изменить форматирование к некоторым экземплярам поля даты и времени.

Откройте *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* файл и замените существующий код следующим:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Это приводит к полной даты и времени, для отображения и добавляет класс CSS, заставив текст зеленого и большой.

Откройте *Movie.cs* файл и добавьте [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут `ReleaseDate` свойства, как показано в следующем примере:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Это означает, что ASP.NET MVC, при его отображении `ReleaseDate` свойство (в частности, а не просто любой `DateTime` объекта), следует использовать *LoudDateTime.cshtml* шаблона.

Нажмите CTRL+F5, чтобы запустить приложение.

Обратите внимание, что `ReleaseDate` свойство теперь отображает дату и время крупным шрифтом зеленый.

Вернитесь к `UIHint` атрибут в *Movie.cs* файл и закомментируйте его поэтому *LoudDateTime.cshtml* шаблон не будет использоваться. Снова запустите приложение. Дата выпуска отображается большой и зеленый. Проверяет, что *Views\Shared\DisplayTemplates\DateTime.cshtml* шаблон используется в представлениях Index и Details.

Как упоминалось ранее, можно также применить шаблон в представлении, которое позволяет применить шаблон для каждого отдельного экземпляра некоторых данных. Откройте *Views\Movies\Details.cshtml* представления. Добавить `"LoudDateTime"` в качестве второго параметра [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) вызова для `ReleaseDate` поля. Завершенный код выглядит следующим образом:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Это указывает на то, что `LoudDateTime` шаблон должен использоваться для отображения свойства модели, независимо от того, какие атрибуты применяются к модели.

Нажмите CTRL+F5, чтобы запустить приложение.

Убедитесь, что страницы индексов фильмов использует *Views\Shared\DisplayTemplates\DateTime.cshtml* шаблона (красным полужирным шрифтом) и *Movie\Details* странице используется *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* шаблона ("большой" и "зеленый").

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

В следующем разделе вы создадите шаблон для сложного типа.

> [!div class="step-by-step"]
> [Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
