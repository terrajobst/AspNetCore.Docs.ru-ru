---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Использование элемента управления TextBoxWatermark с проверяющими элементами управления (C#) | Документация Майкрософт
author: wenz
description: Элемент управления TextBoxWatermark в AJAX Control Toolkit расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, его я...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 144e4ee8f8096076b4e498844429ebf96e6ea402
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836198"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="942e4-104">Использование элемента управления TextBoxWatermark с проверяющими элементами управления (C#)</span><span class="sxs-lookup"><span data-stu-id="942e4-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="942e4-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="942e4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="942e4-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="942e4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="942e4-107">Элемент управления TextBoxWatermark в AJAX Control Toolkit расширяет текстовое поле для отображения текста в поле.</span><span class="sxs-lookup"><span data-stu-id="942e4-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="942e4-108">Когда пользователь щелкает в поле, оно будет очищено.</span><span class="sxs-lookup"><span data-stu-id="942e4-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="942e4-109">Если пользователь оставляет поле без ввода текста, заданными текст отображается повторно.</span><span class="sxs-lookup"><span data-stu-id="942e4-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="942e4-110">Это может привести с проверяющими элементами управления ASP.NET, на той же странице, но может преодолеть эти проблемы.</span><span class="sxs-lookup"><span data-stu-id="942e4-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="942e4-111">Обзор</span><span class="sxs-lookup"><span data-stu-id="942e4-111">Overview</span></span>

<span data-ttu-id="942e4-112">`TextBoxWatermark` В AJAX Control Toolkit, расширяет текстовое поле для отображения текста в поле.</span><span class="sxs-lookup"><span data-stu-id="942e4-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="942e4-113">Когда пользователь щелкает в поле, оно будет очищено.</span><span class="sxs-lookup"><span data-stu-id="942e4-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="942e4-114">Если пользователь оставляет поле без ввода текста, заданными текст отображается повторно.</span><span class="sxs-lookup"><span data-stu-id="942e4-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="942e4-115">Это может привести с проверяющими элементами управления ASP.NET, на той же странице, но может преодолеть эти проблемы.</span><span class="sxs-lookup"><span data-stu-id="942e4-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="942e4-116">Шаги</span><span class="sxs-lookup"><span data-stu-id="942e4-116">Steps</span></span>

<span data-ttu-id="942e4-117">Базовая настройка примера является следующее: `TextBox` водяными знаками элемента управления с помощью `TextBoxWatermarkExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="942e4-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="942e4-118">Кнопка запускает обратную передачу и позже будет использоваться для активации элементов управления проверки на странице.</span><span class="sxs-lookup"><span data-stu-id="942e4-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="942e4-119">Кроме того `ScriptManager` управления необходим для инициализации ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="942e4-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="942e4-120">Теперь добавьте `RequiredFieldValidator` элемент управления, который проверяет, существует ли текстовое поле при отправке формы.</span><span class="sxs-lookup"><span data-stu-id="942e4-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="942e4-121">`InitialValue` Свойства проверяющего элемента управления должно быть присвоено значение, которое используется в `TextBoxWatermarkExtender` управления: при отправке формы неизменными текстовое значение значение предела в ней:</span><span class="sxs-lookup"><span data-stu-id="942e4-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="942e4-122">Тем не менее есть одна проблема, при таком подходе: Если клиент отключает JavaScript, поле не автоматически заполняемом с текстом водяного знака, поэтому `RequiredFieldValidator` не вызывает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="942e4-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="942e4-123">Таким образом, второй `RequiredFieldValidator` управления необходим, который проверяет наличие пустом текстовом поле (пропуск `InitialValue` атрибут).</span><span class="sxs-lookup"><span data-stu-id="942e4-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="942e4-124">Так как оба проверяющие элементы управления используют `Display` = `"Dynamic"`, конечный пользователь не может отличить от внешнего вида, какой из двух проверяющие элементы управления произошло; вместо этого, похоже, возникла только один из них.</span><span class="sxs-lookup"><span data-stu-id="942e4-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="942e4-125">Наконец добавьте код на стороне сервера для вывода текст в поле, если не проверяющий элемент управления выдаваться сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="942e4-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="942e4-126">[![Проверяющий элемент управления сообщает, что отсутствует текст в поле](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="942e4-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="942e4-127">Проверяющий элемент управления сообщает, что в поле отсутствует текст ([Просмотр полноразмерного изображения](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="942e4-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="942e4-128">[Назад](using-textboxwatermark-in-a-formview-cs.md)
> [Вперед](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="942e4-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
