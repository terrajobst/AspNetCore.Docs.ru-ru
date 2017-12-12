---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: "С помощью расширителя управления ColorPicker (C#) | Документы Microsoft"
author: microsoft
description: "ColorPicker является расширитель ASP.NET AJAX, который предоставляет клиентские функции выбора цвета с пользовательским Интерфейсом в элементе управления всплывающее окно. Она может быть присоединена к любой ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3cde9552e8aecd5e7e651a825902fb79ae108c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-colorpicker-control-extender-c"></a>С помощью расширителя управления ColorPicker (C#)
====================
по [Microsoft](https://github.com/microsoft)

> ColorPicker является расширитель ASP.NET AJAX, который предоставляет клиентские функции выбора цвета с пользовательским Интерфейсом в элементе управления всплывающее окно. Она может быть присоединена к любому элементу управления ASP.NET TextBox. Его.


Целью данного учебника является объясняется, как можно использовать элемент управления расширителя ColorPicker набор средств управления AJAX. Расширитель элемента управления ColorPicker отображается всплывающее окно, в котором можно выбрать цвет. ColorPicker полезно каждый раз, когда вы хотите предоставить интуитивно понятный пользовательский интерфейс для пользователя для выбора цвета.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Расширение управления TextBox с ColorPicker управления расширителем

Например, представьте себе, вы хотите создать веб-сайт, позволяющий посетителей создать настраиваемые визитные карточки. Посетители могут введите текст для визитной карточкой и выберите цвет. Страницы ASP.NET в листинге 1 содержит два элемента управления TextBox с именем txtCardText и txtCardColor. При отправке формы отображаются выбранные значения (см. рис. 1).


[![Простая форма для создания визитные карточки](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**На рисунке 01**: простая форма для создания визитной карточкой ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Листинг 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Форма в список 1 работает, но он не обеспечивает удобную работу пользователя. Пользователь должен ввести цвет в текстовом поле. Если пользователю специализированные цвет - например, просто правой оттенок зеленого pea - то пользователь необходимо понять, код цвета HTML без помочь.

Расширитель ColorPicker элемента управления можно использовать для создания улучшить взаимодействие с пользователем. Компонент ColorPicker отображается в диалоговом окне выбора цвета при перемещении фокуса в элемент управления TextBox (см. рис. 2).


[![Расширение управления ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**На рисунке 02**: ColorPicker управления расширителем ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image4.png))


Необходимо выполнить два шага, чтобы использовать расширитель ColorPicker элемента управления с формой в листинге 1:

1. Добавить на страницу элемент управления ScriptManager
2. Добавить на страницу управления расширителя ColorPicker

Перед использованием ColorPicker, необходимо добавить наличия ScriptManager на странице. Является удобным инструментом для добавления ScriptManager прямо под открывающей серверную &lt;формы&gt; тег. ScriptManager можно перетащить на страницу из области элементов (ScriptManager находится на вкладке расширения AJAX). Кроме того можно ввести следующий тег в представление источника под открывающего тега формы на стороне сервера:

&lt;ASP: ScriptManager ID = «ScriptManager1» runat = «server» /&gt;

Самый простой способ добавить на страницу управления расширителя ColorPicker находится в режиме конструктора. Отображается при наведении мыши txtCardColor TextBox параметр одноименное включает можно добавить расширение (см. рис. 3). Если вы выберете этот параметр, откроется мастер расширения (см. рис. 4).


[![Добавление расширения](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**На рисунке 03**: Добавление расширитель ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![При выборе расширитель элемента управления с помощью мастера расширения](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**На рисунке 04**: Выбор расширитель элемента управления с помощью мастера расширения ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image8.png))


Можно выбрать ColorPicker расширения для расширения txtCardColor TextBox с ColorPicker расширителя. Нажмите кнопку ОК, чтобы закрыть диалоговое окно.

После внесения этих изменений исходной страницы выглядит как список 2.

Листинг 2 - CreateCard.aspx (с ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Обратите внимание, что страница теперь содержит элемент управления ColorPickerExtender прямо под txtCardColor управления TextBox. ColorPickerExtender расширяет txtCardColor управления, чтобы он отображает диалоговое окно выбора цвета.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>С помощью кнопки, чтобы открыть диалоговое окно выбора цвета

Расширитель ColorPicker поддерживает следующие свойства:

- PopupButtonId - идентификатор кнопки на странице, которая отображается в диалоговом окне выбора цвета.
- PopupPosition - позицию относительно целевого элемента управления, а диалогового окна выбора цвета. Возможными значениями являются абсолютное, центр, слева снизу, BottomRight, TopLeft, справа сверху, справа и влево (значение по умолчанию — слева снизу).
- SampleControlId - идентификатор элемента управления, который отображает выбранный цвет.
- SelectedColor экземпляра ColorPicker - начальный цвет, выбранный по ColorPicker.

Эти свойства можно использовать для настройки отображения диалогового окна выбора цвета и способ отображения выбранного цвета. Страницы в списке 3 показано, как можно использовать несколько из этих свойств.

**Листинг 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

На странице в списке 3 содержит Выбор цветов кнопку (см. рис. 5). При нажатии этой кнопки, диалоговое окно выбора цвета появляется над текстового поля. Если выбрать цвет из диалогового окна выбранный цвет отображается как цвет фона lblSample управления Label.

Свойство ColorPicker PopupButtonID используется для связи с расширителя ColorPicker кнопку выбора цвета. При указании значения для свойства PopupButtonID диалоговое окно выбора цвета больше не отображается, когда целевой элемент управления имеет фокус. Необходимо нажать кнопку для отображения диалогового окна.

Чтобы связать элемент управления, который отображает выбранный цвет с ColorPicker используется свойство SampleControlID. ColorPicker изменяет цвет фона данного элемента управления для выбранного цвета.


[![Отображение диалогового окна выбора цвета с кнопкой](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**На рисунке 05**: Отображение диалогового окна выбора цвета с кнопкой ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Сводка

В этом учебнике вы узнали, как использовать расширитель ColorPicker элемента управления для отображения всплывающее диалоговое окно выбора цвета. Во-первых мы рассмотрели, как отобразить диалоговое окно при перемещении фокуса на элемент управления TextBox. Далее вы узнали, как создать кнопку, которая отображает диалоговое окно выбора цвета при нажатии кнопки.

>[!div class="step-by-step"]
[Вперед](using-the-colorpicker-control-extender-vb.md)
