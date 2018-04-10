---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: С помощью набора средств управления AJAX и расширителей элементов управления (C#) | Документы Microsoft
author: microsoft
description: Дополнительные сведения о добавлении элементов управления для элементов управления AJAX и расширителей для страниц ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>С помощью набора средств управления AJAX и расширителей элементов управления (C#)
====================
по [Microsoft](https://github.com/microsoft)

> Дополнительные сведения о добавлении элементов управления для элементов управления AJAX и расширителей для страниц ASP.NET.


Набор элементов управления AJAX содержит набор элементов управления и расширителей элементов управления. В этом краткий учебник вы узнаете, как добавление элементов управления и расширителей элементов управления на страницу ASP.NET.

> [!NOTE] 
> 
> Инструкции по установке набора элементов управления AJAX и добавление элементов управления AJAX в область элементов Visual Studio или Visual Web Developer, см. в разделе учебника [Приступая к работе с элементов управления AJAX](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>С помощью набора средств управления AJAX

Элемент управления для элементов управления AJAX работает так же, как обычный элемент управления ASP.NET. Можно перетащить элемент управления из панели элементов на странице ASP.NET. Элемент управления можно добавить на страницу в режиме конструктора или представление источника.

Нет одного специальные требования при использовании элементов управления из набора элементов управления AJAX. Страница должна содержать элемент управления ScriptManager. Элемент управления ScriptManager отвечает для включения всех необходимых элементов управления AJAX элементами управления необходимый код JavaScript.

Например вкладке элементов управления AJAX включает элемент управления с именем управления редактора. Этот элемент управления отображает полнофункциональный редактор HTML. Выполните следующие действия, чтобы добавить на страницу элемент управления редактором.

1. Создайте новую страницу ASP.NET с именем ShowEditor.aspx
2. Выберите элемент управления ScriptManager from beneath вкладки расширения AJAX в области элементов и перетащите элемент управления на странице.
3. Выберите элемент управления редактором from beneath вкладке элементов управления AJAX в области элементов и перетащите элемент управления на страницу (см. рис. 1). Конструктор должна выглядеть как на рис. 2.
4. Запустите веб-сайт, выбрав параметр меню **отладка, начать отладку** или при нажатии клавиши F5.
5. Должна отобразиться страница на рис. 3.


[![Выбрав элемент управления редактора HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Рисунок 01**: при выборе элемента управления редактора HTML ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Конструктор Visual Studio с помощью элемента управления ScriptManager и редактирования](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**На рисунке 02**: конструктор Visual Studio с помощью элемента управления ScriptManager и редактирования ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![На странице DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**На рисунке 03**: страница DisplayEditor.aspx ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>С помощью средств расширения набора средств управления элемент управления AJAX

Набор элементов управления AJAX также содержит расширителей элементов управления. Как и предполагает его имя, расширитель элемента управления расширяет функциональность существующего элемента управления. Например расширения элемента управления ConfirmButton расширяет стандартный элемент управления ASP.NET Button. Расширитель изменяет поведение кнопки управления s, чтобы на кнопке отображается диалоговое окно подтверждения при щелчке.

Расширитель элемента управления, как элемент управления набора элементов управления AJAX необходим элемент управления ScriptManager. Перед началом использования расширителей элементов управления на странице, необходимо добавить элемент управления ScriptManager на страницу.

Выполните следующие действия для использования расширения ConfirmButton элемента управления.

1. Создайте новую страницу ASP.NET с именем ShowConfirmButton.aspx
2. Добавьте элемент управления ScriptManager на страницу, перетащив элемент управления на странице from beneath вкладки расширения AJAX.
3. Стандартный элемент управления Button на страницу можно добавьте, перетащив кнопку from beneath вкладка "Стандартные" в панели элементов в область конструктора.
4. Нажмите кнопку **Добавить расширитель** задач параметр (см. рис. 4).
5. В диалоговом окне выберите расширитель выберите ConfirmButtonExtender (см. рис. 5) и нажмите кнопку "ОК".
6. Выберите элемент управления в конструкторе и разверните расширителей Button1\_ConfirmButtonExtender узел в окне «Свойства» (см. рис. 6). Присвойте значение *действительно?* ConfirmText свойству.
7. Запустите страницу, выбрав параметр меню **отладка, начать отладку** или нажмите клавишу F5.


[![Параметр задачи Добавить расширитель](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**На рисунке 04**: параметр задачи Добавить расширитель ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![При выборе расширения элемента управления ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**На рисунке 05**: Выбор расширения элемента управления ConfirmButton ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![Задание свойства ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**На рисунке 06**: задание свойства ConfirmButton ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


При открытии страницы, вы увидите кнопку. При нажатии кнопки появляется диалоговое окно подтверждения на рис. 7.


[![Отображение диалогового окна подтверждения](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**На рисунке 07**: отображение диалоговое окно подтверждения ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Обратите внимание, что обычно не перетащить расширитель элемента управления на страницу. Вместо этого использовать **Добавить расширитель** задач параметр, чтобы добавить расширение к элементу управления, уже был добавлен на страницу. Кроме того, обратите внимание, значение элемента управления свойства модуля, открыв страницу свойств для расширяемого элемента.

В одном элементе управления ASP.NET может быть расширена путем расширителей несколько элементов управления. Окно свойств для элемента управления расширяемого отображает список всех расширений элемента управления, связанный с элементом управления.

> [!div class="step-by-step"]
> [Назад](get-started-with-the-ajax-control-toolkit-cs.md)
> [Вперед](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
