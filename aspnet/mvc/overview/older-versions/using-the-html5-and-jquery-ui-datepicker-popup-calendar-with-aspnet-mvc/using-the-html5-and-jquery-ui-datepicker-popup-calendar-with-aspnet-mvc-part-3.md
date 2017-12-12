---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 3 | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: dc81961094928025e25cf62ce4d51d12bc67b80c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Использование HTML5 и раскрывающегося календаря с выбором дат jQuery с ASP.NET MVC — часть 3
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основные принципы работы с редактор шаблонов, шаблоны отображения и пользовательского интерфейса jQuery datepicker раскрывающегося календаря в приложении MVC ASP.NET.


## <a name="working-with-complex-types"></a>Работа со сложными типами

В этом разделе будет создать класс адрес и сведения о создании шаблона для его отображения.

В *моделей* папки, создайте новый файл класса с именем *Person.cs* размещения двух типов: `Person` класса и `Address` класса. `Person` Класс будет содержать свойство, которое имеет тип `Address`. `Address` Тип — это сложный тип, то есть он не является одним из встроенных типов, таких как `int`, `string`, или `double`. Вместо этого он имеет несколько свойств. Код для новых классов выглядит следующим образом:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

В `Movie` контроллера, добавьте следующий `PersonDetail` действие для отображения экземпляр пользователь:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Затем добавьте следующий код в `Movie` контроллера для заполнения `Person` модель с образцами данных:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Откройте *Views\Movies\PersonDetail.cshtml* и добавьте следующую разметку для файла `PersonDetail` представления.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Нажмите клавиши Ctrl + F5 для запуска приложения и перейдите к *фильмов/PersonDetail*.

`PersonDetail` Представление не содержит `Address` сложный тип, как показано на этом снимке экрана. (Адрес не отображается).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Модели данных не отображаются, так как это сложный тип. Для отображения информации об адресе, откройте *Views\Movies\PersonDetail.cshtml* попытку и добавьте следующую разметку.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Полную разметку для `PersonDetail` теперь представление выглядит следующим образом:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Снова запустите приложение и отображения `PersonDetail` представления. Сведения об адресе теперь отображается:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Создание шаблона для комплексного типа

В этом разделе вы создадите шаблон, который будет использоваться для подготовки к просмотру `Address` сложного типа. При создании шаблона для `Address` типа, ASP.NET MVC можно автоматически использовать его для форматирования модели адресов в любом месте приложения. Это дает возможность управлять отрисовку `Address` тип в одном месте, в приложении.

В *Views\Shared\DisplayTemplates* папки, создайте строго типизированным представлением с именем **адрес**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Нажмите кнопку **добавить**, а затем откройте новый *Views\Shared\DisplayTemplates\Address.cshtml* файла. Новое представление содержит созданный следующую разметку:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Запустите приложение и отображения `PersonDetail` представления. На этот раз `Address` только что созданный шаблон используется для отображения `Address` сложный тип, поэтому отображение выглядит следующим образом:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Сводка: Способов для указания формата отображения для модели и шаблона

Вы видели, что можно указать формат или шаблон для свойства модели с помощью следующих подходов:

- Применение `DisplayFormat` атрибут свойства в модели. Например приведенный ниже код вызывает даты для отображения без времени:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Применение [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) для свойства в модели, а также указать тип данных атрибута. Например приведенный ниже код вызывает даты для отображения без времени.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Если приложение содержит *date.cshtml* шаблона в *Views\Shared\DisplayTemplates* папки или *Views\Movies\DisplayTemplates* папки, этот шаблон будет использоваться для подготовки к просмотру `DateTime` свойство. В противном случае встроенную систему шаблонов ASP.NET отображает свойство как дата.
- Создание шаблона экрана в *Views\Shared\DisplayTemplates* папки или *Views\Movies\DisplayTemplates* папки, имя которого совпадает с типом данных, которую требуется отформатировать. Например, вы видели, *Views\Shared\DisplayTemplates\DateTime.cshtml* использовался для подготовки к просмотру `DateTime` свойства в модели, без добавления атрибута в модель и Добавление разметки представления.
- С помощью [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) атрибут для модели, чтобы указать шаблон для отображения свойства модели.
- Явное добавление отображаемое имя шаблона для [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) вызвать в представлении.

Используется подход зависит от того, что нужно делать в приложении. Довольно часто для подхода для получения именно такие форматирование, необходимый.

В следующем разделе мы перейдем немного рассказ и перемещение из Настройка способа отображения для настройки способа ввода данных. Будете подключать datepicker jQuery с представлениями изменения в приложение, чтобы предоставить называется способ указания даты.

>[!div class="step-by-step"]
[Назад](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Вперед](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
