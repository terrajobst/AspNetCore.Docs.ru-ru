---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Создание пользовательских AJAX элемента управления набор средств расширения (Visual Basic) | Документы Microsoft
author: microsoft
description: Пользовательские расширения позволяют настроить и расширить возможности элементов управления ASP.NET без необходимости создавать новые классы.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 06950770bf788fff4a03e9d41fd448ea675a8bce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="f317a-103">Создание набора средств управления расширитель AJAX пользовательского элемента управления (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="f317a-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="f317a-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f317a-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f317a-105">Пользовательские расширения позволяют настроить и расширить возможности элементов управления ASP.NET без необходимости создавать новые классы.</span><span class="sxs-lookup"><span data-stu-id="f317a-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="f317a-106">В этом учебнике вы научитесь создавать расширитель пользовательских элементов управления AJAX-элемента управления.</span><span class="sxs-lookup"><span data-stu-id="f317a-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="f317a-107">Мы создадим простой, но полезным, новый расширений, который изменяет состояние кнопки из отключенного состояния во включенное при вводе текста в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="f317a-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="f317a-108">После прочтения этого учебника, можно расширить набор помощи собственных средств управления расширения AJAX ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f317a-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="f317a-109">Можно создать расширителей пользовательского элемента управления с помощью Visual Studio или Visual Web Developer (Убедитесь, что имеется последнюю версию Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="f317a-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="f317a-110">Общие сведения о модуле DisabledButton</span><span class="sxs-lookup"><span data-stu-id="f317a-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="f317a-111">Наш новый элемент управления расширителя называется DisabledButton расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="f317a-112">Этого расширителя будет иметь три свойства:</span><span class="sxs-lookup"><span data-stu-id="f317a-112">This extender will have three properties:</span></span>

- <span data-ttu-id="f317a-113">TargetControlID - текстовое поле, которое расширяет элемент управления.</span><span class="sxs-lookup"><span data-stu-id="f317a-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="f317a-114">TargetButtonIID - кнопки, отключен или включен.</span><span class="sxs-lookup"><span data-stu-id="f317a-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="f317a-115">DisabledText - текст, который отображается на кнопке.</span><span class="sxs-lookup"><span data-stu-id="f317a-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="f317a-116">При вводе, кнопки отображает значение свойства текст кнопки.</span><span class="sxs-lookup"><span data-stu-id="f317a-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="f317a-117">Можно подключить расширитель DisabledButton к элементу управления текстового поля и кнопку.</span><span class="sxs-lookup"><span data-stu-id="f317a-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="f317a-118">Перед тем как любой текст, кнопка становится недоступной, и текстовое поле и кнопку выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f317a-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="f317a-119">([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="f317a-120">После начала ввода текста, эта кнопка включена, и текстовое поле и кнопку выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f317a-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="f317a-121">([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="f317a-122">Для создания нашей расширитель элемента управления, необходимо создать следующие три файла:</span><span class="sxs-lookup"><span data-stu-id="f317a-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="f317a-123">DisabledButtonExtender.vb - этот файл является класса серверный элемент управления, который будет управлять Создание медиаприставки и позволяют задавать свойства во время разработки.</span><span class="sxs-lookup"><span data-stu-id="f317a-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="f317a-124">Он также определяет свойства, которые могут быть установлены на вашего расширения.</span><span class="sxs-lookup"><span data-stu-id="f317a-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="f317a-125">Эти свойства доступны через код, а также во время разработки и соответствуют свойствам, определенным в файле DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="f317a-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="f317a-126">DisabledButtonBehavior.js--Этот файл является добавляется вся логика сценария вашего клиента.</span><span class="sxs-lookup"><span data-stu-id="f317a-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="f317a-127">DisabledButtonDesigner.vb - этот класс включает функциональные возможности времени разработки.</span><span class="sxs-lookup"><span data-stu-id="f317a-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="f317a-128">Этот класс требуется в том случае, если требуется, чтобы элемент управления расширителя для правильной работы с Visual Studio или Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="f317a-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="f317a-129">Поэтому расширитель элемента управления состоит из серверный элемент управления, поведение клиентских и серверных конструктора класса.</span><span class="sxs-lookup"><span data-stu-id="f317a-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="f317a-130">Вы узнаете, как создать все три из этих файлов в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="f317a-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="f317a-131">Создание настраиваемого расширения веб-сайта и проект</span><span class="sxs-lookup"><span data-stu-id="f317a-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="f317a-132">Первым шагом является создание проекта библиотеки классов и веб-сайта в Visual Studio или Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="f317a-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="f317a-133">Мы ll создания пользовательских расширений в проекте библиотеки классов и тестирования пользовательских расширений в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="f317a-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="f317a-134">Разрешить s начинаться с веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="f317a-134">Let�s start with the website.</span></span> <span data-ttu-id="f317a-135">Выполните следующие действия для создания веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="f317a-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="f317a-136">Выберите пункт меню в **файл, создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="f317a-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="f317a-137">Выберите **веб-сайт ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="f317a-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="f317a-138">Назовите новый веб-сайт *Website1*.</span><span class="sxs-lookup"><span data-stu-id="f317a-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="f317a-139">Нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="f317a-139">Click the **OK** button.</span></span>

<span data-ttu-id="f317a-140">Далее необходимо создать проект библиотеки классов, который будет содержать код для управления расширителя:</span><span class="sxs-lookup"><span data-stu-id="f317a-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="f317a-141">Выберите пункт меню в **файл, добавить новый проект**.</span><span class="sxs-lookup"><span data-stu-id="f317a-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="f317a-142">Выберите **библиотеки классов** шаблона.</span><span class="sxs-lookup"><span data-stu-id="f317a-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="f317a-143">Назовите новую библиотеку классов с именем **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="f317a-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="f317a-144">Нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="f317a-144">Click the **OK** button.</span></span>

<span data-ttu-id="f317a-145">После выполнения этих действий в окне обозревателя решений должна выглядеть как на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="f317a-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="f317a-146">[![Решения с использованием проекта библиотеки веб-сайт и класса](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="f317a-147">**На рисунке 01**: решение с проектом библиотеки веб-сайта и класс ([щелкните, чтобы просмотреть полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="f317a-148">Далее необходимо добавить все ссылки на необходимые сборки в проект библиотеки классов:</span><span class="sxs-lookup"><span data-stu-id="f317a-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="f317a-149">Щелкните правой кнопкой мыши проект CustomExtenders и выберите пункт меню в **добавить ссылку**.</span><span class="sxs-lookup"><span data-stu-id="f317a-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="f317a-150">Перейдите на вкладку .NET.</span><span class="sxs-lookup"><span data-stu-id="f317a-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="f317a-151">Добавьте ссылки на следующие сборки:</span><span class="sxs-lookup"><span data-stu-id="f317a-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="f317a-152">System.Web.dll.</span><span class="sxs-lookup"><span data-stu-id="f317a-152">System.Web.dll</span></span>
    2. <span data-ttu-id="f317a-153">System.Web.Extensions.dll.</span><span class="sxs-lookup"><span data-stu-id="f317a-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="f317a-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="f317a-154">System.Design.dll</span></span>
    4. <span data-ttu-id="f317a-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="f317a-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="f317a-156">Выберите вкладку "Обзор".</span><span class="sxs-lookup"><span data-stu-id="f317a-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="f317a-157">Добавьте ссылку на файл AjaxControlToolkit.dll сборку.</span><span class="sxs-lookup"><span data-stu-id="f317a-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="f317a-158">Эта сборка находится в папке, куда вы загрузили элементов управления AJAX.</span><span class="sxs-lookup"><span data-stu-id="f317a-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="f317a-159">Чтобы узнать, были добавлены все ссылки справа, щелкнув правой кнопкой мыши проект, выбрав свойства и ссылки на вкладку (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="f317a-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="f317a-160">[![Ссылки на папку с необходимые ссылки](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="f317a-161">**Рисунок 02**: папка ссылок с необходимые ссылки ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="f317a-162">Создание пользовательского элемента управления расширителем</span><span class="sxs-lookup"><span data-stu-id="f317a-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="f317a-163">Теперь, когда у нас есть нашей библиотеки классов, мы можно приступать к созданию элемента-расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="f317a-164">Разрешить s начинаться с костей bare класса элемента управления пользовательские расширения (см. список 1).</span><span class="sxs-lookup"><span data-stu-id="f317a-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="f317a-165">**Листинг 1 - MyCustomExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="f317a-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="f317a-166">Существует несколько вещей, которые можно заметить о класс элемента управления расширителем в список 1.</span><span class="sxs-lookup"><span data-stu-id="f317a-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="f317a-167">Во-первых Обратите внимание, что класс наследует от базового класса ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="f317a-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="f317a-168">Все элементы управления расширителем элементов управления AJAX являются производными от этого базового класса.</span><span class="sxs-lookup"><span data-stu-id="f317a-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="f317a-169">Например базовый класс содержит TargetID свойство, которое является обязательным свойством расширитель каждого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="f317a-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="f317a-170">Далее Обратите внимание, что класс включает следующие два атрибута, связанные с клиентского скрипта:</span><span class="sxs-lookup"><span data-stu-id="f317a-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="f317a-171">В результате WebResource - файл в качестве внедренного ресурса в сборку.</span><span class="sxs-lookup"><span data-stu-id="f317a-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="f317a-172">В результате ClientScriptResource - сценария ресурса должно быть извлечено из сборки.</span><span class="sxs-lookup"><span data-stu-id="f317a-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="f317a-173">Указав используется для внедрения в сборку файла MyControlBehavior.js JavaScript, при компиляции пользовательских расширений.</span><span class="sxs-lookup"><span data-stu-id="f317a-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="f317a-174">Атрибут ClientScriptResource используется для получения сценария MyControlBehavior.js из сборки при использовании пользовательских расширений в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="f317a-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="f317a-175">Чтобы WebResource и ClientScriptResource атрибуты для работы необходимо скомпилировать файл JavaScript в качестве внедренного ресурса.</span><span class="sxs-lookup"><span data-stu-id="f317a-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="f317a-176">В окне обозревателя решений выберите файл, откройте окно свойств и присвоить значение *внедренный ресурс* для **действие при построении** свойство.</span><span class="sxs-lookup"><span data-stu-id="f317a-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="f317a-177">Обратите внимание, что расширитель элемента управления также включает атрибут TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="f317a-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="f317a-178">Этот атрибут используется для указания типа элемента управления, который расширяется за счет управления расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="f317a-179">В случае список 1 расширитель элемента управления используется для расширения элемента управления TextBox.</span><span class="sxs-lookup"><span data-stu-id="f317a-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="f317a-180">И, наконец Обратите внимание, что пользовательские расширения содержит свойство с именем MyProperty.</span><span class="sxs-lookup"><span data-stu-id="f317a-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="f317a-181">Свойство помечено атрибутом ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="f317a-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="f317a-182">Методы GetPropertyValue() и SetPropertyValue() используются для передачи значения свойства из серверного элемента управления расширителем поведение клиентского.</span><span class="sxs-lookup"><span data-stu-id="f317a-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="f317a-183">Разрешить s пойти дальше и реализация кода для наших DisabledButton расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="f317a-184">Код для данного объекта расширения можно найти в списке 2.</span><span class="sxs-lookup"><span data-stu-id="f317a-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="f317a-185">**Листинг 2 - DisabledButtonExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="f317a-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="f317a-186">Расширитель DisabledButton в списке 2 имеет два свойства с именем TargetButtonID и DisabledText.</span><span class="sxs-lookup"><span data-stu-id="f317a-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="f317a-187">IDReferenceProperty, применяемой к свойству TargetButtonID предотвращает назначение ничего, кроме идентификатора элемента управления Button для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="f317a-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="f317a-188">Атрибуты WebResource и ClientScriptResource связать клиентского поведения, расположенных в файле с именем DisabledButtonBehavior.js с этого расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="f317a-189">Мы рассмотрим этот файл JavaScript в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="f317a-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="f317a-190">Создание пользовательских расширений поведения</span><span class="sxs-lookup"><span data-stu-id="f317a-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="f317a-191">Компонент клиентских расширений управления называется поведение.</span><span class="sxs-lookup"><span data-stu-id="f317a-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="f317a-192">В поведении DisabledButton содержится фактической логики для отключения и включения кнопки.</span><span class="sxs-lookup"><span data-stu-id="f317a-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="f317a-193">В списке 3 включается код JavaScript для поведения.</span><span class="sxs-lookup"><span data-stu-id="f317a-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="f317a-194">**Листинг 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="f317a-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="f317a-195">Файл JavaScript в списке 3 содержит клиентский класс с именем DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="f317a-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="f317a-196">Этот класс, как его две серверные включает два свойства с именем TargetButtonID и получить к нему можно получить с помощью DisabledText\_TargetButtonID/set\_TargetButtonID и получить\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="f317a-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="f317a-197">Метод initialize() связывает с обработчиком событий keyup с целевой элемент поведения.</span><span class="sxs-lookup"><span data-stu-id="f317a-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="f317a-198">Каждый раз, введите букву в текстовое поле, связанное с этим поведением keyup обработчик выполняет.</span><span class="sxs-lookup"><span data-stu-id="f317a-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="f317a-199">Обработчик keyup включает или отключает кнопку в зависимости от того, содержит ли текстовое поле, связанных с данным поведением любой текст.</span><span class="sxs-lookup"><span data-stu-id="f317a-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="f317a-200">Помните, что необходимо скомпилировать файл JavaScript в списке 3 в качестве внедренного ресурса.</span><span class="sxs-lookup"><span data-stu-id="f317a-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="f317a-201">В окне обозревателя решений выберите файл, откройте окно свойств и присвоить значение *внедренный ресурс* для **действие при построении** свойство (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="f317a-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="f317a-202">Этот параметр доступен в Visual Studio и Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="f317a-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="f317a-203">[![Добавление файла JavaScript в качестве внедренного ресурса](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="f317a-204">**На рисунке 03**: Добавление файла JavaScript в качестве внедренного ресурса ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="f317a-205">Создание пользовательского расширяющего конструктора</span><span class="sxs-lookup"><span data-stu-id="f317a-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="f317a-206">Имеется один последний класс, необходимо создать для завершения нашей расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="f317a-207">Необходимо создать класс конструктора в листинге 4.</span><span class="sxs-lookup"><span data-stu-id="f317a-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="f317a-208">Этот класс обязательно расширителя правильно работать с Visual Studio или Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="f317a-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="f317a-209">**Листинг 4 - DisabledButtonDesigner.vb**</span><span class="sxs-lookup"><span data-stu-id="f317a-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="f317a-210">Связать конструктора в листинге 4 с DisabledButton расширитель с помощью атрибута конструктора. Необходимо применить атрибут конструктор в класс DisabledButtonExtender следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f317a-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="f317a-211">С помощью пользовательских расширений</span><span class="sxs-lookup"><span data-stu-id="f317a-211">Using the Custom Extender</span></span>

<span data-ttu-id="f317a-212">Теперь, когда мы завершения создания расширения элемента управления DisabledButton, пришло время для их использования в наш веб-узла ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f317a-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="f317a-213">Во-первых необходимо добавить настраиваемые расширения в область элементов.</span><span class="sxs-lookup"><span data-stu-id="f317a-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="f317a-214">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f317a-214">Follow these steps:</span></span>

1. <span data-ttu-id="f317a-215">Откройте страницу ASP.NET, дважды щелкнув страницы в окне обозревателя решений.</span><span class="sxs-lookup"><span data-stu-id="f317a-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="f317a-216">Щелкните правой кнопкой мыши область элементов и выберите пункт меню в **Выбор элементов**.</span><span class="sxs-lookup"><span data-stu-id="f317a-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="f317a-217">В диалоговом окне Выбор элементов панели элементов перейдите к сборке CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="f317a-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="f317a-218">Нажмите кнопку **ОК** кнопку, чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="f317a-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="f317a-219">После выполнения этих шагов DisabledButton управления расширителя должен отображаться в панели элементов (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="f317a-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="f317a-220">[![DisabledButton на панели инструментов](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="f317a-221">**На рисунке 04**: DisabledButton на панели инструментов ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="f317a-222">Теперь нам нужны для создания новой страницы ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f317a-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="f317a-223">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f317a-223">Follow these steps:</span></span>

1. <span data-ttu-id="f317a-224">Создайте новую страницу ASP.NET с именем ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="f317a-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="f317a-225">Перетащите наличия ScriptManager на странице.</span><span class="sxs-lookup"><span data-stu-id="f317a-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="f317a-226">Перетащите элемент управления TextBox на страницу.</span><span class="sxs-lookup"><span data-stu-id="f317a-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="f317a-227">Перетащите элемент управления Button на страницу.</span><span class="sxs-lookup"><span data-stu-id="f317a-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="f317a-228">В окне свойств измените значение свойства Button ID <em>btnSave</em> и свойству Text значение *Сохранить\**.</span><span class="sxs-lookup"><span data-stu-id="f317a-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="f317a-229">Мы создали страницу стандартных элементов управления ASP.NET текстовое поле и кнопку.</span><span class="sxs-lookup"><span data-stu-id="f317a-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="f317a-230">Далее нам нужно расширить элемент управления TextBox со DisabledButton расширителя:</span><span class="sxs-lookup"><span data-stu-id="f317a-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="f317a-231">Выберите **Добавить расширитель** задач, чтобы открыть диалоговое окно мастера расширения (см. рис. 5).</span><span class="sxs-lookup"><span data-stu-id="f317a-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="f317a-232">Обратите внимание, что у диалогового окна есть наших пользовательских расширений DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="f317a-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="f317a-233">Выберите расширитель DisabledButton и нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="f317a-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="f317a-234">[![Диалоговое окно мастера расширения](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="f317a-235">**На рисунке 05**: диалоговое окно мастера расширителя ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="f317a-236">Наконец мы можно задать свойства DisabledButton расширителя.</span><span class="sxs-lookup"><span data-stu-id="f317a-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="f317a-237">Можно изменить свойства расширителя DisabledButton путем изменения свойств элемента управления TextBox:</span><span class="sxs-lookup"><span data-stu-id="f317a-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="f317a-238">Выберите текстовое поле в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="f317a-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="f317a-239">В окне «Свойства» разверните узел расширения (см. рис. 6).</span><span class="sxs-lookup"><span data-stu-id="f317a-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="f317a-240">Присвойте значение *Сохранить* DisabledText свойства и значения *btnSave* TargetButtonID свойству.</span><span class="sxs-lookup"><span data-stu-id="f317a-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="f317a-241">[![Настройка свойств расширителя](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="f317a-242">**На рисунке 06**: задание свойств расширителя ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="f317a-243">При выполнении страницы (с нажимать клавишу F5), изначально отключен элемент управления Button.</span><span class="sxs-lookup"><span data-stu-id="f317a-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="f317a-244">Сразу запустить, введя текст в текстовое поле, элемент управления является кнопка (см. рис. 7).</span><span class="sxs-lookup"><span data-stu-id="f317a-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="f317a-245">[![Расширитель DisabledButton в действии](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="f317a-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="f317a-246">**На рисунке 07**: DisabledButton расширений в действии ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="f317a-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="f317a-247">Сводка</span><span class="sxs-lookup"><span data-stu-id="f317a-247">Summary</span></span>

<span data-ttu-id="f317a-248">Целью данного учебника было объясняется, как можно расширить набор элементов управления AJAX с помощью пользовательских расширений элементов управления.</span><span class="sxs-lookup"><span data-stu-id="f317a-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="f317a-249">В этом учебнике мы создали простого расширения элемента управления DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="f317a-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="f317a-250">Мы реализовали этого расширителя путем создания класса DisabledButtonExtender, поведение DisabledButtonBehavior JavaScript и класс DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="f317a-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="f317a-251">Выполните аналогичный набор шагов, каждый раз при создании пользовательского элемента управления расширителем.</span><span class="sxs-lookup"><span data-stu-id="f317a-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f317a-252">Назад</span><span class="sxs-lookup"><span data-stu-id="f317a-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
