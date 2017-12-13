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
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="5cbe5-104">С помощью расширителя управления ColorPicker (C#)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="5cbe5-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5cbe5-106">ColorPicker является расширитель ASP.NET AJAX, который предоставляет клиентские функции выбора цвета с пользовательским Интерфейсом в элементе управления всплывающее окно.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="5cbe5-107">Она может быть присоединена к любому элементу управления ASP.NET TextBox.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="5cbe5-108">Его.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-108">It.</span></span>


<span data-ttu-id="5cbe5-109">Целью данного учебника является объясняется, как можно использовать элемент управления расширителя ColorPicker набор средств управления AJAX.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="5cbe5-110">Расширитель элемента управления ColorPicker отображается всплывающее окно, в котором можно выбрать цвет.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="5cbe5-111">ColorPicker полезно каждый раз, когда вы хотите предоставить интуитивно понятный пользовательский интерфейс для пользователя для выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="5cbe5-112">Расширение управления TextBox с ColorPicker управления расширителем</span><span class="sxs-lookup"><span data-stu-id="5cbe5-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="5cbe5-113">Например, представьте себе, вы хотите создать веб-сайт, позволяющий посетителей создать настраиваемые визитные карточки.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="5cbe5-114">Посетители могут введите текст для визитной карточкой и выберите цвет.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="5cbe5-115">Страницы ASP.NET в листинге 1 содержит два элемента управления TextBox с именем txtCardText и txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="5cbe5-116">При отправке формы отображаются выбранные значения (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="5cbe5-117">[![Простая форма для создания визитные карточки](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="5cbe5-118">**На рисунке 01**: простая форма для создания визитной карточкой ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5cbe5-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="5cbe5-119">**Листинг 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="5cbe5-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="5cbe5-120">Форма в список 1 работает, но он не обеспечивает удобную работу пользователя.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="5cbe5-121">Пользователь должен ввести цвет в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="5cbe5-122">Если пользователю специализированные цвет - например, просто правой оттенок зеленого pea - то пользователь необходимо понять, код цвета HTML без помочь.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="5cbe5-123">Расширитель ColorPicker элемента управления можно использовать для создания улучшить взаимодействие с пользователем.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="5cbe5-124">Компонент ColorPicker отображается в диалоговом окне выбора цвета при перемещении фокуса в элемент управления TextBox (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="5cbe5-125">[![Расширение управления ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="5cbe5-126">**На рисунке 02**: ColorPicker управления расширителем ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="5cbe5-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="5cbe5-127">Необходимо выполнить два шага, чтобы использовать расширитель ColorPicker элемента управления с формой в листинге 1:</span><span class="sxs-lookup"><span data-stu-id="5cbe5-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="5cbe5-128">Добавить на страницу элемент управления ScriptManager</span><span class="sxs-lookup"><span data-stu-id="5cbe5-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="5cbe5-129">Добавить на страницу управления расширителя ColorPicker</span><span class="sxs-lookup"><span data-stu-id="5cbe5-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="5cbe5-130">Перед использованием ColorPicker, необходимо добавить наличия ScriptManager на странице.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="5cbe5-131">Является удобным инструментом для добавления ScriptManager прямо под открывающей серверную &lt;формы&gt; тег.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="5cbe5-132">ScriptManager можно перетащить на страницу из области элементов (ScriptManager находится на вкладке расширения AJAX).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="5cbe5-133">Кроме того можно ввести следующий тег в представление источника под открывающего тега формы на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="5cbe5-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="5cbe5-134">&lt;ASP: ScriptManager ID = «ScriptManager1» runat = «server» /&gt;</span><span class="sxs-lookup"><span data-stu-id="5cbe5-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="5cbe5-135">Самый простой способ добавить на страницу управления расширителя ColorPicker находится в режиме конструктора.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="5cbe5-136">Отображается при наведении мыши txtCardColor TextBox параметр одноименное включает можно добавить расширение (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="5cbe5-137">Если вы выберете этот параметр, откроется мастер расширения (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="5cbe5-138">[![Добавление расширения](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="5cbe5-139">**На рисунке 03**: Добавление расширитель ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5cbe5-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="5cbe5-140">[![При выборе расширитель элемента управления с помощью мастера расширения](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="5cbe5-141">**На рисунке 04**: Выбор расширитель элемента управления с помощью мастера расширения ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="5cbe5-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="5cbe5-142">Можно выбрать ColorPicker расширения для расширения txtCardColor TextBox с ColorPicker расширителя.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="5cbe5-143">Нажмите кнопку ОК, чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="5cbe5-144">После внесения этих изменений исходной страницы выглядит как список 2.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="5cbe5-145">Листинг 2 - CreateCard.aspx (с ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="5cbe5-146">Обратите внимание, что страница теперь содержит элемент управления ColorPickerExtender прямо под txtCardColor управления TextBox.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="5cbe5-147">ColorPickerExtender расширяет txtCardColor управления, чтобы он отображает диалоговое окно выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="5cbe5-148">С помощью кнопки, чтобы открыть диалоговое окно выбора цвета</span><span class="sxs-lookup"><span data-stu-id="5cbe5-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="5cbe5-149">Расширитель ColorPicker поддерживает следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5cbe5-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="5cbe5-150">PopupButtonId - идентификатор кнопки на странице, которая отображается в диалоговом окне выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="5cbe5-151">PopupPosition - позицию относительно целевого элемента управления, а диалогового окна выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="5cbe5-152">Возможными значениями являются абсолютное, центр, слева снизу, BottomRight, TopLeft, справа сверху, справа и влево (значение по умолчанию — слева снизу).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="5cbe5-153">SampleControlId - идентификатор элемента управления, который отображает выбранный цвет.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="5cbe5-154">SelectedColor экземпляра ColorPicker - начальный цвет, выбранный по ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="5cbe5-155">Эти свойства можно использовать для настройки отображения диалогового окна выбора цвета и способ отображения выбранного цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="5cbe5-156">Страницы в списке 3 показано, как можно использовать несколько из этих свойств.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="5cbe5-157">**Листинг 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="5cbe5-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="5cbe5-158">На странице в списке 3 содержит Выбор цветов кнопку (см. рис. 5).</span><span class="sxs-lookup"><span data-stu-id="5cbe5-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="5cbe5-159">При нажатии этой кнопки, диалоговое окно выбора цвета появляется над текстового поля.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="5cbe5-160">Если выбрать цвет из диалогового окна выбранный цвет отображается как цвет фона lblSample управления Label.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="5cbe5-161">Свойство ColorPicker PopupButtonID используется для связи с расширителя ColorPicker кнопку выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="5cbe5-162">При указании значения для свойства PopupButtonID диалоговое окно выбора цвета больше не отображается, когда целевой элемент управления имеет фокус.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="5cbe5-163">Необходимо нажать кнопку для отображения диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="5cbe5-164">Чтобы связать элемент управления, который отображает выбранный цвет с ColorPicker используется свойство SampleControlID.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="5cbe5-165">ColorPicker изменяет цвет фона данного элемента управления для выбранного цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="5cbe5-166">[![Отображение диалогового окна выбора цвета с кнопкой](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="5cbe5-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="5cbe5-167">**На рисунке 05**: Отображение диалогового окна выбора цвета с кнопкой ([Просмотр полноразмерное изображение](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="5cbe5-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="5cbe5-168">Сводка</span><span class="sxs-lookup"><span data-stu-id="5cbe5-168">Summary</span></span>

<span data-ttu-id="5cbe5-169">В этом учебнике вы узнали, как использовать расширитель ColorPicker элемента управления для отображения всплывающее диалоговое окно выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="5cbe5-170">Во-первых мы рассмотрели, как отобразить диалоговое окно при перемещении фокуса на элемент управления TextBox.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="5cbe5-171">Далее вы узнали, как создать кнопку, которая отображает диалоговое окно выбора цвета при нажатии кнопки.</span><span class="sxs-lookup"><span data-stu-id="5cbe5-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5cbe5-172">Вперед</span><span class="sxs-lookup"><span data-stu-id="5cbe5-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
