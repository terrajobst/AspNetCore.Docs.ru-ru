---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 2 | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 2
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в приложении MVC ASP.NET.


## <a name="adding-an-automatic-datetime-template"></a>Добавление шаблона даты и времени автоматического

В первой части этого учебника было показано, как можно добавить атрибуты модели, чтобы явно указать форматирование и как можно явно указать шаблон, который используется для отображения модели. Например [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) в следующий код явно задает форматирование для `ReleaseDate` свойства.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

В следующем примере [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибута с помощью `Date` перечисление, указывает, что шаблон даты следует использовать для отображения модели. Если в проекте отсутствует шаблон даты, используется шаблон даты.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Тем не менее ASP. MVC можно выполнить сопоставление типов с помощью соглашение over конфигурации путем поиска для шаблона, который соответствует имени типа. Это позволяет создавать шаблон, который автоматически форматирует данные без использования во всех атрибутов или кода. Для этой части учебника вы создадите шаблон, который автоматически применяется к свойствам модели типа [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Не требуется использовать атрибут или для других конфигураций, чтобы указать, что шаблон должен использоваться для отображения всех свойств типа модели [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Вы также узнаете способ настроить внешний вид отдельных свойств или даже отдельные поля.

Чтобы начать, давайте удалить существующие сведения о форматировании и полной даты отображались в приложении.

Откройте *Movie.cs* файла и закомментируйте `DataType` атрибут `ReleaseDate` свойства:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Нажмите CTRL+F5, чтобы запустить приложение.

Обратите внимание, что `ReleaseDate` теперь отображает дату и время, так как это значение по умолчанию, когда нет сведения о форматировании предоставляется.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Добавление стилей CSS для тестирования новых шаблонов

Перед созданием шаблона для форматирования дат, мы добавим несколько правил стилей CSS, которые можно применить в новые шаблоны. Это поможет вам определить, что новый шаблон использует отображаемой страницы.

Откройте *Content\Site.cs*s и добавьте следующие правила CSS в конец файла:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Добавление шаблонов отображения даты и времени

Теперь можно создать новый шаблон. В *Views\Movies* папке создайте *DisplayTemplates* папки.

В *представления\общие* папки, создать *DisplayTemplates* папки и *EditorTemplates* папки.

Шаблоны отображения в *Views\Shared\DisplayTemplates* папка будет использоваться всеми контроллерами. Шаблоны отображения в *Views\Movie\DisplayTemplates* папка будет использоваться только `Movie` контроллера. (Если шаблон с тем же именем присутствует в обеих папках шаблона в *Views\Movie\DisplayTemplates* папки — то есть более конкретный шаблон, имеет более высокий приоритет для представлений, возвращенных `Movie` контроллера.)

В **обозревателе решений**, разверните *представления* папку, разверните *Shared* папку, а затем щелкните правой кнопкой мыши *Views\Shared\DisplayTemplates* папки.

Нажмите кнопку **добавить** и нажмите кнопку **представление**. **Добавить представление** диалоговое окно.

В **имя представления** введите `DateTime`. (Это имя необходимо использовать для поиска соответствия имя типа.)

Выберите **создать как частичное представление** флажок. Убедитесь, что **макета или главная страница** и **создать строго типизированное представление** не установлены флажки.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Нажмите кнопку **Добавить**. Объект *DateTime.cshtml* шаблон создается в *Views\Shared\DisplayTemplates*.

На следующем рисунке показана *представления* папки в **обозревателе решений** после `DateTime` создаются шаблоны отображения и редактор.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Откройте *Views\Shared\DisplayTemplates\DateTime.cshtml* и добавьте следующую разметку, которая использует [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) метод для форматирования свойства как дата без времени. ( `{0:d}` Формат указывает на короткий формат даты.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Повторите этот шаг, чтобы создать `DateTime` шаблона в *Views\Movie\DisplayTemplates* папки. Используйте следующий код в *Views\Movie\DisplayTemplates\DateTime.cshtml* файла.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` Класс CSS вызывает даты для отображения красным полужирным шрифтом. Вы добавили `loud-1` класс CSS только в качестве временной меры, чтобы можно было легко видеть при использовании этого конкретного шаблона.

Вы выполнили создается и пользовательские шаблоны, которые ASP.NET будет использовать для отображения дат. Более общие шаблона (в *Views\Shared\DisplayTemplates* папки) отображает простой короткого формата даты. Шаблон, которому специально для `Movie` контроллера (в *Views\Movies\DisplayTemplates* папки) отображает краткий, также форматируется в виде красным полужирным шрифтом.

Нажмите CTRL+F5, чтобы запустить приложение. Браузер отображает представление Index для приложения.

`ReleaseDate` Теперь отображаются полужирным красным шрифтом без времени. Это помогает проверить, `DateTime` Шаблонизированный вспомогательный объект в *Views\Movies\DisplayTemplates* папка выбрана по `DateTime` Шаблонизированный вспомогательный объект в общей папке (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Теперь Переименовать *Views\Movies\DisplayTemplates\DateTime.cshtml* файл *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Нажмите CTRL+F5, чтобы запустить приложение.

Это время `ReleaseDate` свойство отображается дата без времени и без полужирным красным шрифтом. Это показывает, что введите шаблон, который имеет имя данных (в данном случае `DateTime`) автоматически используется для отображения всех свойств модели этого типа. По окончании переименован *DateTime.cshtml* файл *LoudDateTime.cshtml*, ASP.NET не удается найти шаблон в *Views\Movies\DisplayTemplates* папки, чтобы он использовался *DateTime.cshtml* шаблона из * Views\Movies\Shared\* папки.

(Шаблон, соответствующий учитывается регистр, поэтому можно было создать имя файла шаблона с любой регистр. Например *DATETIME.chstml, datetime.cshtml*, и *DaTeTiMe.cshtml* будет соответствовать всем `DateTime` тип.)

Для просмотра: на этом этапе `ReleaseDate` поле отображается с помощью *Views\Movies\DisplayTemplates\DateTime.cshtml* шаблона, который отображает данные в формате короткой даты, но в противном случае добавляет не специальный формат.

### <a name="using-uihint-to-specify-a-display-template"></a>Использование UIHint для указания шаблона отображения

Если веб-приложение содержит много `DateTime` поля и по умолчанию будет отображаться в формате только даты, все или большинство из них *DateTime.cshtml* шаблона является хорошим подходом. Но что делать, если имеется несколько дат место для отображения полной даты и времени? Без проблем. Можно создавать дополнительные шаблоны и использовать [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут, чтобы задать формат для полной даты и времени. Затем можно выборочно применять этот шаблон. Можно использовать [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут на уровне модели, либо можно указать шаблон внутри представления. В этом разделе вы увидите, как использовать `UIHint` атрибут, чтобы выборочно менять параметры форматирования для поля даты и времени для некоторых экземпляров.

Откройте *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* файл и замените существующий код следующим:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Это вызывает полной даты и времени для отображения и добавляет класс CSS, который делает текст зеленый и большой.

Откройте *Movie.cs* и добавьте [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут `ReleaseDate` свойства, как показано в следующем примере:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Сообщает ASP.NET MVC, при отображении `ReleaseDate` свойство (в частности, а не любые `DateTime` объекта), следует использовать *LoudDateTime.cshtml* шаблона.

Нажмите CTRL+F5, чтобы запустить приложение.

Обратите внимание, что `ReleaseDate` свойство теперь отображает дату и время крупным шрифтом зеленый.

Вернитесь к `UIHint` атрибута в *Movie.cs* файла и закомментируйте его таким образом *LoudDateTime.cshtml* не будет использоваться шаблон. Снова запустите приложение. Дата выпуска не отображается, большими и зеленый. Проверяет, что *Views\Shared\DisplayTemplates\DateTime.cshtml* шаблон используется в индексе и сведения о представлениях.

Как упоминалось ранее, также можно применить шаблон в представлении, которое позволяет применить шаблон к экземпляру отдельных некоторые данные. Откройте *Views\Movies\Details.cshtml* представления. Добавить `"LoudDateTime"` в качестве второго параметра [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) вызова для `ReleaseDate` поля. Завершенный код выглядит следующим образом:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Указывает, что `LoudDateTime` следует использовать шаблон для отображения свойства модели, независимо от того, какие атрибуты будут применены к модели.

Нажмите CTRL+F5, чтобы запустить приложение.

Убедитесь, что страницы индекса фильма использует *Views\Shared\DisplayTemplates\DateTime.cshtml* шаблона (красным полужирным шрифтом) и *Movie\Details* странице используется *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* шаблона (большой и зеленый).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

В следующем разделе вы создадите шаблон для сложного типа.

> [!div class="step-by-step"]
> [Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
