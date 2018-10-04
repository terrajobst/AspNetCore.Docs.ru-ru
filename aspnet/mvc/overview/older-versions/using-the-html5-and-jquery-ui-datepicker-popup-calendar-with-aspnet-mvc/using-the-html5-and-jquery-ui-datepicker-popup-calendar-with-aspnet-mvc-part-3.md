---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Использование HTML5 и элемент интерфейса всплывающего календаря jQuery в ASP.NET MVC. часть 3 | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник поможет основные сведения о работе с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: baaca74d584a6d5028824e877c561494cdff044e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577227"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Использование HTML5 и элемент интерфейса всплывающего календаря jQuery в ASP.NET MVC. часть 3
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> Этом учебнике описываются основы работы с помощью редактора шаблонов, шаблоны отображения и jQuery элемент интерфейса всплывающего календаря в приложении ASP.NET MVC.


## <a name="working-with-complex-types"></a>Работа со сложными типами

В этом разделе будет создать класс адрес и узнайте, как создать шаблон, чтобы отобразить ее.

В *моделей* папки, создайте новый файл класса с именем *Person.cs* размещения двух типов: `Person` класс и `Address` класса. `Person` Класс содержит свойства, который типизируется как `Address`. `Address` Тип — сложный тип, то есть он не является одним из встроенных типов, таких как `int`, `string`, или `double`. Вместо этого он имеет несколько свойств. Код для новых классов выглядит следующим образом:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

В `Movie` контроллера, добавьте следующий `PersonDetail` действие для отображения экземпляра person:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Затем добавьте следующий код, чтобы `Movie` контроллера для заполнения `Person` модель с образцами данных:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Откройте *Views\Movies\PersonDetail.cshtml* файл и добавьте следующую разметку для `PersonDetail` представления.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Нажмите клавиши Ctrl + F5, чтобы запустить приложение и перейдите к *Movies/PersonDetail*.

`PersonDetail` Представление не содержит `Address` сложного типа, как показано на следующем снимке экрана. (Адрес не отображается).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Модели данные не отображаются, так как это сложный тип. Для отображения информации об адресе, откройте *Views\Movies\PersonDetail.cshtml* снова и добавьте следующую разметку.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Полную разметку для `PersonDetail` теперь представление выглядит следующим образом:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Снова запустите приложение и отображения `PersonDetail` представления. Сведения об адресах теперь отображается:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Создание шаблона для комплексного типа

В этом разделе вы создадите шаблон, который будет использоваться для подготовки к просмотру `Address` сложного типа. При создании шаблона для `Address` типа, ASP.NET MVC может автоматически использовать его для форматирования модель адрес в любом месте приложения. Это дает возможность управлять отрисовку `Address` тип из лишь в одном месте в приложении.

В *Views\Shared\DisplayTemplates* папки, создайте строго типизированным представлением с именем **адрес**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Нажмите кнопку **добавить**, а затем откройте новый *Views\Shared\DisplayTemplates\Address.cshtml* файла. Новое представление содержит созданный следующую разметку:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Запустите приложение и отображения `PersonDetail` представления. На этот раз `Address` шаблон, который вы только что создали, используется для отображения `Address` сложный тип, поэтому отображение выглядит следующим образом:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Сводка: Способов для указания формата отображения для модели и шаблона

Вы уже видели, что можно указать формат или шаблон для свойства модели с помощью следующих подходов:

- Применение `DisplayFormat` атрибут к свойству в модели. Например следующий код вызывает Дата, которая будет отображаться без времени:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Применение [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибут к свойству в модели, а также указать тип данных. Например следующий код вызывает Дата, которая будет отображаться без времени.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Если приложение содержит *date.cshtml* шаблона в *Views\Shared\DisplayTemplates* папку или *Views\Movies\DisplayTemplates* папка, этот шаблон будет использоваться для подготовки к просмотру `DateTime` свойство. В противном случае встроенных системных шаблонов ASP.NET отображает свойство как дата.
- Создание шаблона отображения в *Views\Shared\DisplayTemplates* папку или *Views\Movies\DisplayTemplates* папку, имя которого совпадает с типом данных, необходимо отформатировать. Например, вы увидели, что *Views\Shared\DisplayTemplates\DateTime.cshtml* был использован для подготовки к просмотру `DateTime` свойства в модели, без добавления атрибута в модель и Добавление разметки в представлениях.
- С помощью [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут модели, чтобы указать шаблон для отображения свойства модели.
- Явное добавление отображаемое имя шаблона для [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) вызвать в представлении.

Подход, который можно использовать зависит от того, что необходимо сделать в приложении. Нередко смешивания этих подходов для получения именно для такого форматирование, вам потребуется.

В следующем разделе мы перейдем смягчении немного и перемещение из Настройка, способ отображения данных для настройки способа ввода. Будет подключить datepicker jQuery с представлениями редактирования в приложении, чтобы обеспечить хитрый способ для указания даты.

> [!div class="step-by-step"]
> [Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
