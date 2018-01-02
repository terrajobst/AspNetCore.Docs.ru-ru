---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "С помощью набора средств управления AJAX и расширителей элементов управления (Visual Basic) | Документы Microsoft"
author: microsoft
description: "Дополнительные сведения о добавлении элементов управления для элементов управления AJAX и расширителей для страниц ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="45652-103">С помощью набора средств управления AJAX и расширителей элементов управления (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="45652-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="45652-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="45652-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="45652-105">Дополнительные сведения о добавлении элементов управления для элементов управления AJAX и расширителей для страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="45652-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="45652-106">Набор элементов управления AJAX содержит набор элементов управления и расширителей элементов управления.</span><span class="sxs-lookup"><span data-stu-id="45652-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="45652-107">В этом краткий учебник вы узнаете, как добавление элементов управления и расширителей элементов управления на страницу ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="45652-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="45652-108">Инструкции по установке набора элементов управления AJAX и добавление элементов управления AJAX в область элементов Visual Studio или Visual Web Developer, см. в разделе учебника [Приступая к работе с элементов управления AJAX](get-started-with-the-ajax-control-toolkit-vb.md).</span><span class="sxs-lookup"><span data-stu-id="45652-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="45652-109">С помощью набора средств управления AJAX</span><span class="sxs-lookup"><span data-stu-id="45652-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="45652-110">Элемент управления для элементов управления AJAX работает так же, как обычный элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="45652-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="45652-111">Можно перетащить элемент управления из панели элементов на странице ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="45652-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="45652-112">Элемент управления можно добавить на страницу в режиме конструктора или представление источника.</span><span class="sxs-lookup"><span data-stu-id="45652-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="45652-113">Нет одного специальные требования при использовании элементов управления из набора элементов управления AJAX.</span><span class="sxs-lookup"><span data-stu-id="45652-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="45652-114">Страница должна содержать элемент управления ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="45652-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="45652-115">Элемент управления ScriptManager отвечает для включения всех необходимых элементов управления AJAX элементами управления необходимый код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="45652-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="45652-116">Например вкладке элементов управления AJAX включает элемент управления с именем управления редактора.</span><span class="sxs-lookup"><span data-stu-id="45652-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="45652-117">Этот элемент управления отображает полнофункциональный редактор HTML.</span><span class="sxs-lookup"><span data-stu-id="45652-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="45652-118">Выполните следующие действия, чтобы добавить на страницу элемент управления редактором.</span><span class="sxs-lookup"><span data-stu-id="45652-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="45652-119">Создайте новую страницу ASP.NET с именем ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="45652-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="45652-120">Выберите элемент управления ScriptManager from beneath вкладки расширения AJAX в области элементов и перетащите элемент управления на странице.</span><span class="sxs-lookup"><span data-stu-id="45652-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="45652-121">Выберите элемент управления редактором from beneath вкладке элементов управления AJAX в области элементов и перетащите элемент управления на страницу (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="45652-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="45652-122">Конструктор должна выглядеть как на рис. 2.</span><span class="sxs-lookup"><span data-stu-id="45652-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="45652-123">Запустите веб-сайт, выбрав параметр меню **отладка, начать отладку** или при нажатии клавиши F5.</span><span class="sxs-lookup"><span data-stu-id="45652-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="45652-124">Должна отобразиться страница на рис. 3.</span><span class="sxs-lookup"><span data-stu-id="45652-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="45652-125">[![Выбрав элемент управления редактора HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45652-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="45652-126">**Рисунок 01**: при выборе элемента управления редактора HTML ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="45652-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="45652-127">[![Конструктор Visual Studio с помощью элемента управления ScriptManager и редактирования](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="45652-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="45652-128">**На рисунке 02**: конструктор Visual Studio с помощью элемента управления ScriptManager и редактирования ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="45652-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="45652-129">[![На странице DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="45652-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="45652-130">**На рисунке 03**: страница DisplayEditor.aspx ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="45652-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="45652-131">С помощью средств расширения набора средств управления элемент управления AJAX</span><span class="sxs-lookup"><span data-stu-id="45652-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="45652-132">Набор элементов управления AJAX также содержит расширителей элементов управления.</span><span class="sxs-lookup"><span data-stu-id="45652-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="45652-133">Как и предполагает его имя, расширитель элемента управления расширяет функциональность существующего элемента управления.</span><span class="sxs-lookup"><span data-stu-id="45652-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="45652-134">Например расширения элемента управления ConfirmButton расширяет стандартный элемент управления ASP.NET Button.</span><span class="sxs-lookup"><span data-stu-id="45652-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="45652-135">Расширитель изменяет поведение кнопки управления s, чтобы на кнопке отображается диалоговое окно подтверждения при щелчке.</span><span class="sxs-lookup"><span data-stu-id="45652-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="45652-136">Расширитель элемента управления, как элемент управления набора элементов управления AJAX необходим элемент управления ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="45652-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="45652-137">Перед началом использования расширителей элементов управления на странице, необходимо добавить элемент управления ScriptManager на страницу.</span><span class="sxs-lookup"><span data-stu-id="45652-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="45652-138">Выполните следующие действия для использования расширения ConfirmButton элемента управления.</span><span class="sxs-lookup"><span data-stu-id="45652-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="45652-139">Создайте новую страницу ASP.NET с именем ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="45652-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="45652-140">Добавьте элемент управления ScriptManager на страницу, перетащив элемент управления на странице from beneath вкладки расширения AJAX.</span><span class="sxs-lookup"><span data-stu-id="45652-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="45652-141">Стандартный элемент управления Button на страницу можно добавьте, перетащив кнопку from beneath вкладка "Стандартные" в панели элементов в область конструктора.</span><span class="sxs-lookup"><span data-stu-id="45652-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="45652-142">Нажмите кнопку **Добавить расширитель** задач параметр (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="45652-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="45652-143">В диалоговом окне выберите расширитель выберите ConfirmButtonExtender (см. рис. 5) и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="45652-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="45652-144">Выберите элемент управления в конструкторе и разверните расширителей Button1\_ConfirmButtonExtender узел в окне «Свойства» (см. рис. 6).</span><span class="sxs-lookup"><span data-stu-id="45652-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="45652-145">Присвойте значение *действительно?* ConfirmText свойству.</span><span class="sxs-lookup"><span data-stu-id="45652-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="45652-146">Запустите страницу, выбрав параметр меню **отладка, начать отладку** или нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="45652-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="45652-147">[![Параметр задачи Добавить расширитель](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="45652-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="45652-148">**На рисунке 04**: параметр задачи Добавить расширитель ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="45652-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="45652-149">[![При выборе расширения элемента управления ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="45652-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="45652-150">**На рисунке 05**: Выбор расширения элемента управления ConfirmButton ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="45652-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="45652-151">[![Задание свойства ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="45652-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="45652-152">**На рисунке 06**: задание свойства ConfirmButton ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="45652-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="45652-153">При открытии страницы, вы увидите кнопку.</span><span class="sxs-lookup"><span data-stu-id="45652-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="45652-154">При нажатии кнопки появляется диалоговое окно подтверждения на рис. 7.</span><span class="sxs-lookup"><span data-stu-id="45652-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="45652-155">[![Отображение диалогового окна подтверждения](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="45652-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="45652-156">**На рисунке 07**: отображение диалоговое окно подтверждения ([Просмотр полноразмерное изображение](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="45652-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="45652-157">Обратите внимание, что обычно не перетащить расширитель элемента управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="45652-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="45652-158">Вместо этого использовать **Добавить расширитель** задач параметр, чтобы добавить расширение к элементу управления, уже был добавлен на страницу.</span><span class="sxs-lookup"><span data-stu-id="45652-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="45652-159">Кроме того, обратите внимание, значение элемента управления свойства модуля, открыв страницу свойств для расширяемого элемента.</span><span class="sxs-lookup"><span data-stu-id="45652-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="45652-160">В одном элементе управления ASP.NET может быть расширена путем расширителей несколько элементов управления.</span><span class="sxs-lookup"><span data-stu-id="45652-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="45652-161">Окно свойств для элемента управления расширяемого отображает список всех расширений элемента управления, связанный с элементом управления.</span><span class="sxs-lookup"><span data-stu-id="45652-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="45652-162">[Назад](get-started-with-the-ajax-control-toolkit-vb.md)
[Вперед](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="45652-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
