---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: С помощью расширитель элемента управления ColorPicker (C#) | Документация Майкрософт
author: microsoft
description: ColorPicker является расширитель ASP.NET AJAX, который предоставляет функции выбора цвета на стороне клиента с помощью пользовательского интерфейса в элементе управления всплывающего окна. Его можно подключить к любой ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b8d581ed426227ed77435e22c84e9ea5d62ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838523"
---
<a name="using-the-colorpicker-control-extender-c"></a>С помощью расширитель элемента управления ColorPicker (C#)
====================
по [Microsoft](https://github.com/microsoft)

> ColorPicker является расширитель ASP.NET AJAX, который предоставляет функции выбора цвета на стороне клиента с помощью пользовательского интерфейса в элементе управления всплывающего окна. Его можно подключить к любому элементу управления ASP.NET TextBox. Его.


Чтобы объяснить, как используется расширитель элемента управления ColorPicker набор средств управления AJAX является целью данного учебника. Расширитель элемента управления ColorPicker отображается всплывающее диалоговое окно, которое позволяет выбрать цвет. ColorPicker параметр полезен, если вы хотите предоставить интуитивный пользовательский интерфейс пользователя для выбора цвета.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Расширение управления TextBox с расширитель элемента управления ColorPicker

Представьте, например, что вы хотите создать веб-сайт, позволяющий посетителям Создание настроенного визитные карточки. Посетители могут введите текст для бизнес-Карточка и выбрать цвет. Страницы ASP.NET в листинге 1 содержит два элемента управления TextBox с именем txtCardText и txtCardColor. При отправке формы отображаются выбранные значения (см. рис. 1).


[![Простая форма для создания бизнес-карточка](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Рис 01**: простая форма для создания бизнес-карточка ([Просмотр полноразмерного изображения](using-the-colorpicker-control-extender-cs/_static/image2.png))


**В листинге 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Формы в листинге 1 работает, но он не обеспечивает удобную работу пользователя. Пользователь должен ввести цвет в текстовом поле. Если пользователь хочет специализированные color – например, просто правой оттенок green рабочее время —, а затем пользователь должны понять, код цвета HTML без любое Справка.

Расширитель элемента управления ColorPicker можно использовать для создания удобства работы пользователя. ColorPicker отображает диалоговое окно цвет при перемещении фокуса в элементе управления TextBox (см. рис. 2).


[![Расширитель элемента управления ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Рис. 02**: расширитель элемента управления ColorPicker ([Просмотр полноразмерного изображения](using-the-colorpicker-control-extender-cs/_static/image4.png))


Необходимо выполнить два шага, чтобы использовать расширитель элемента управления ColorPicker с формой в листинге 1:

1. Добавление элемента управления ScriptManager к странице
2. Добавьте на страницу расширитель элемента управления ColorPicker

Перед использованием ColorPicker, ScriptManager необходимо добавить на страницу. — Хорошее место для добавления ScriptManager сразу под открывающей серверных &lt;формы&gt; тега. ScriptManager можно перетащить на страницу из области элементов (ScriptManager находится на вкладке расширений AJAX). Кроме того можно ввести следующий тег в представление источника под открывающего тега формы на стороне сервера:

&lt;ASP: ScriptManager ID = «ScriptManager1» runat = «server» /&gt;

Самый простой способ добавить расширитель элемента управления ColorPicker страницы находится в режиме конструктора. Если навести указатель мыши txtCardColor TextBox одноименное появилась позволяет добавить расширитель (см. рис. 3). Если вы выберете этот параметр, откроется мастер расширения (см. рис. 4).


[![Добавление расширителя](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Рис 03**: Добавление расширитель ([Просмотр полноразмерного изображения](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Выбрав расширитель элемента управления с помощью мастера расширений](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Рис. 04**: Выбор расширитель элемента управления с помощью мастера расширений ([Просмотр полноразмерного изображения](using-the-colorpicker-control-extender-cs/_static/image8.png))


Вы можете выбрать ColorPicker расширителя для расширения txtCardColor TextBox с ColorPicker расширителя. Нажмите кнопку ОК, чтобы закрыть диалоговое окно.

После внесения этих изменений, исходный код страницы выглядит как листинг 2.

В листинге 2 - CreateCard.aspx (с ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Обратите внимание на то, что страница теперь содержит элемент управления ColorPickerExtender прямо под txtCardColor элемент управления TextBox. ColorPickerExtender расширяет txtCardColor управления таким образом, чтобы он отображает диалоговое окно выбора цвета.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>С помощью кнопки, чтобы открыть диалоговое окно выбора цвета

Расширитель ColorPicker поддерживает следующие свойства:

- PopupButtonId - идентификатор кнопки на страницу, которая вызывает диалоговое окно выбора цвета для отображения.
- PopupPosition - положение, относительно конечный элемент управления, диалоговое окно выбора цвета. Возможными значениями являются абсолютное, Center, слева снизу, BottomRight, TopLeft, справа сверху, справа и слева (по умолчанию — слева снизу).
- SampleControlId - идентификатор элемента управления, который отображает выбранный цвет.
- SelectedColor - банке ColorPicker цвет.

Эти свойства можно использовать для настройки отображения диалоговое окно выбора цвета и способ отображения выбранного цвета. На странице в листинге 3 показано, как можно использовать несколько из этих свойств.

**Листинг 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

На странице в листинге 3 включает в себя Выбор цвета кнопки (см. рис. 5). После нажатия этой кнопки, диалоговое окно выбора цвета появляется над текстового поля. Если вы выбираете цвет в диалоговом окне выбранного цвета отображается как цвет фона lblSample элемент управления Label.

Свойство ColorPicker PopupButtonID используется должен быть сопоставлен расширителя ColorPicker кнопку выбора цвета. При указании значения для свойства PopupButtonID диалоговое окно выбора цвета больше не отображается, когда целевой элемент управления имеет фокус. Необходимо нажать кнопку для отображения диалогового окна.

Свойство SampleControlID позволяет связать элемент управления, который отображает выбранный цвет с ColorPicker. Цвет фона этого элемента управления ColorPicker примет выбранный цвет.


[![Отображение диалоговое окно выбора цвета с кнопкой](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**05 рис**: отображение диалоговое окно выбора цвета с кнопкой ([Просмотр полноразмерного изображения](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Сводка

В этом руководстве вы узнали, как использовать расширитель элемента управления ColorPicker, чтобы отобразить всплывающее диалоговое окно выбора цвета. Во-первых мы рассмотрели, как можно отобразить диалоговое окно, при перемещении фокуса на элементе управления TextBox. Далее вы узнали, как создать кнопку, которая отображает диалоговое окно выбора цвета, при нажатии кнопки.

> [!div class="step-by-step"]
> [Вперед](using-the-colorpicker-control-extender-vb.md)
