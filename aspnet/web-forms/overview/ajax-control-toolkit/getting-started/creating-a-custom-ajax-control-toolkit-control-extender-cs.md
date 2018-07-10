---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Создание пользовательских AJAX элемента управления набора средств расширения (C#) | Документация Майкрософт
author: microsoft
description: Пользовательские расширения позволяют настроить и расширить возможности элементов управления ASP.NET без создания новых классов.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 525975c9e0bddc6ef539b477e9f5db3cfd7ee8c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814269"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="14a46-103">Создание пользовательских AJAX Control Toolkit управления Extender (C#)</span><span class="sxs-lookup"><span data-stu-id="14a46-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="14a46-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="14a46-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="14a46-105">Пользовательские расширения позволяют настроить и расширить возможности элементов управления ASP.NET без создания новых классов.</span><span class="sxs-lookup"><span data-stu-id="14a46-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="14a46-106">В этом руководстве вы узнаете, как создать пользовательский расширяющий элемент управления AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="14a46-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="14a46-107">Мы создадим простой, но полезное, новый расширитель, который изменяет состояние кнопки из отключенного состояния во включенное при вводе текста в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="14a46-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="14a46-108">После изучения этого учебника, можно расширить набор AJAX ASP.NET с помощью собственных управляющих элементов-расширителей.</span><span class="sxs-lookup"><span data-stu-id="14a46-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="14a46-109">Можно создать пользовательский элемент управления расширения с помощью Visual Studio или Visual Web Developer (Убедитесь, что у вас есть последняя версия Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="14a46-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="14a46-110">Общие сведения о модуле DisabledButton</span><span class="sxs-lookup"><span data-stu-id="14a46-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="14a46-111">Наш новый расширитель элемента управления с именем DisabledButton расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="14a46-112">Этого расширителя будет иметь три свойства:</span><span class="sxs-lookup"><span data-stu-id="14a46-112">This extender will have three properties:</span></span>

- <span data-ttu-id="14a46-113">TargetControlID - текстовое поле, которое расширяет элемент управления.</span><span class="sxs-lookup"><span data-stu-id="14a46-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="14a46-114">TargetButtonIID - кнопки, включен или выключен.</span><span class="sxs-lookup"><span data-stu-id="14a46-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="14a46-115">DisabledText - текст, который отображается на кнопке.</span><span class="sxs-lookup"><span data-stu-id="14a46-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="14a46-116">При вводе, кнопка отображает значение свойства текста кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="14a46-117">Можно подключить расширителя DisabledButton к элементу управления текстового поля и кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="14a46-118">Прежде чем ввести любой текст, кнопка отключена, и текстовое поле и кнопку выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="14a46-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="14a46-119">([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="14a46-120">После запуска ввода текста, доступна кнопка и текстовое поле и кнопку выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="14a46-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="14a46-121">([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="14a46-122">Чтобы создать наш расширитель элемента управления, необходимо создать следующие три файла:</span><span class="sxs-lookup"><span data-stu-id="14a46-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="14a46-123">DisabledButtonExtender.cs - этот файл является класс серверного элемента управления, который будет управлять Создание медиаприставки и позволяют задавать свойства во время разработки.</span><span class="sxs-lookup"><span data-stu-id="14a46-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="14a46-124">Он также определяет свойства, которые могут устанавливаться на вашего расширения.</span><span class="sxs-lookup"><span data-stu-id="14a46-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="14a46-125">Эти свойства, доступны через код, а также во время разработки и соответствуют свойствам, определенным в файле DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="14a46-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="14a46-126">DisabledButtonBehavior.js--Этот файл находится в который записывается вся логика сценария вашего клиента.</span><span class="sxs-lookup"><span data-stu-id="14a46-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="14a46-127">DisabledButtonDesigner.cs - этот класс обеспечивает функциональные возможности времени разработки.</span><span class="sxs-lookup"><span data-stu-id="14a46-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="14a46-128">Этот класс требуется в том случае, если вы хотите расширитель элемента управления для правильной работы с Visual Studio или Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="14a46-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="14a46-129">Поэтому управляющего элемента-расширителя состоит из серверного элемента управления, поведения на стороне клиента и класс конструктора на сервере.</span><span class="sxs-lookup"><span data-stu-id="14a46-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="14a46-130">Вы узнаете, как создать все три эти файлы в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="14a46-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="14a46-131">Создание настраиваемого расширения веб-сайта и проекта</span><span class="sxs-lookup"><span data-stu-id="14a46-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="14a46-132">Первым шагом является создание проекта библиотеки классов и веб-сайта в Visual Studio или Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="14a46-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="14a46-133">Мы ll создания пользовательских расширений в проекте библиотеки классов и проверить пользовательского расширителя на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="14a46-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="14a46-134">Позвольте s начинаются с веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14a46-134">Let�s start with the website.</span></span> <span data-ttu-id="14a46-135">Выполните следующие действия для создания веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="14a46-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="14a46-136">Выберите пункт меню **файл, создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="14a46-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="14a46-137">Выберите **веб-сайт ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="14a46-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="14a46-138">Назовите новый веб-сайт *Website1*.</span><span class="sxs-lookup"><span data-stu-id="14a46-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="14a46-139">Нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-139">Click the **OK** button.</span></span>

<span data-ttu-id="14a46-140">Далее нам нужно создать проект библиотеки классов, который будет содержать код для управляющего элемента-расширителя:</span><span class="sxs-lookup"><span data-stu-id="14a46-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="14a46-141">Выберите пункт меню **файл, добавить новый проект**.</span><span class="sxs-lookup"><span data-stu-id="14a46-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="14a46-142">Выберите **библиотеки классов** шаблона.</span><span class="sxs-lookup"><span data-stu-id="14a46-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="14a46-143">Назовите новую библиотеку классов с именем **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="14a46-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="14a46-144">Нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-144">Click the **OK** button.</span></span>

<span data-ttu-id="14a46-145">После выполнения этих действий в окне обозревателя решений должен выглядеть как на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="14a46-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="14a46-146">[![Решение с проектом библиотеки веб-сайт и класса](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="14a46-147">**Рис 01**: решения с проектом библиотеки веб-сайт и класс ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="14a46-148">Далее необходимо добавить все ссылки на сборки, необходимые для проекта библиотеки классов:</span><span class="sxs-lookup"><span data-stu-id="14a46-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="14a46-149">Щелкните правой кнопкой мыши проект CustomExtenders и выберите пункт меню **добавить ссылку**.</span><span class="sxs-lookup"><span data-stu-id="14a46-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="14a46-150">Перейдите на вкладку .NET.</span><span class="sxs-lookup"><span data-stu-id="14a46-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="14a46-151">Добавьте ссылки на следующие сборки:</span><span class="sxs-lookup"><span data-stu-id="14a46-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="14a46-152">System.Web.dll.</span><span class="sxs-lookup"><span data-stu-id="14a46-152">System.Web.dll</span></span>
    2. <span data-ttu-id="14a46-153">System.Web.Extensions.dll.</span><span class="sxs-lookup"><span data-stu-id="14a46-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="14a46-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="14a46-154">System.Design.dll</span></span>
    4. <span data-ttu-id="14a46-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="14a46-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="14a46-156">Перейдите на вкладку обзора.</span><span class="sxs-lookup"><span data-stu-id="14a46-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="14a46-157">Добавьте ссылку на файл AjaxControlToolkit.dll сборку.</span><span class="sxs-lookup"><span data-stu-id="14a46-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="14a46-158">Эта сборка находится в папке, куда вы скачали AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="14a46-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="14a46-159">После выполнения этих действий, папка ссылок проекта библиотеки классов должен выглядеть как на рис. 2.</span><span class="sxs-lookup"><span data-stu-id="14a46-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="14a46-160">[![Папка ссылок с необходимые ссылки](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="14a46-161">**Рис. 02**: папке "ссылки" с необходимые ссылки ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="14a46-162">Создание пользовательского элемента управления расширителя</span><span class="sxs-lookup"><span data-stu-id="14a46-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="14a46-163">Теперь, когда у нас есть нашей библиотеки классов, можно приступать к созданию элемента-расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="14a46-164">Позвольте s начинаются с кости bare класса расширяющего элемента управления (см. в листинге 1).</span><span class="sxs-lookup"><span data-stu-id="14a46-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="14a46-165">**В листинге 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="14a46-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="14a46-166">Существует несколько моментов, которые вы Обратите внимание, что сведения о классе расширитель элемента управления в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="14a46-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="14a46-167">Во-первых Обратите внимание на то, что класс наследует от базового класса ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="14a46-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="14a46-168">Все элементы управления расширителя AJAX Control Toolkit, являются производными от этого базового класса.</span><span class="sxs-lookup"><span data-stu-id="14a46-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="14a46-169">Например базовый класс включает TargetID свойство, которое является обязательным свойством для каждого управляющего элемента-расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="14a46-170">После этого Обратите внимание на то, что класс включает следующие два атрибута, связанные с клиентского скрипта:</span><span class="sxs-lookup"><span data-stu-id="14a46-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="14a46-171">WebResource - вызовет создание файла в качестве внедренного ресурса в сборке.</span><span class="sxs-lookup"><span data-stu-id="14a46-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="14a46-172">ClientScriptResource - приводит к ресурсу скрипта должно быть извлечено из сборки.</span><span class="sxs-lookup"><span data-stu-id="14a46-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="14a46-173">Атрибут WebResource используется для внедрения в сборку файла MyControlBehavior.js JavaScript, при компиляции пользовательского расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="14a46-174">Атрибут ClientScriptResource используется для получения MyControlBehavior.js скрипт из сборки, при использовании пользовательского расширителя на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="14a46-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="14a46-175">Чтобы WebResource и ClientScriptResource атрибуты для работы необходимо скомпилировать файл JavaScript как внедренный ресурс.</span><span class="sxs-lookup"><span data-stu-id="14a46-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="14a46-176">В окне обозревателя решений выберите файл, откройте окно свойств и назначьте *внедренный ресурс* для **действие при построении** свойство.</span><span class="sxs-lookup"><span data-stu-id="14a46-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="14a46-177">Обратите внимание на то, что расширитель элемента управления также включает атрибут TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="14a46-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="14a46-178">Этот атрибут используется для указания типа элемента управления, который расширен с помощью управляющего элемента-расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="14a46-179">В случае листинге 1 расширитель элемента управления используется для расширения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="14a46-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="14a46-180">Наконец Обратите внимание, что пользовательские расширения содержит свойство с именем MyProperty.</span><span class="sxs-lookup"><span data-stu-id="14a46-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="14a46-181">Свойство помечено атрибутом ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="14a46-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="14a46-182">Методы GetPropertyValue() и SetPropertyValue() используются для передачи значения свойства из серверного элемента управления расширителя поведения на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="14a46-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="14a46-183">Позвольте s пойти дальше и реализации кода для наших DisabledButton расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="14a46-184">Код для данного объекта расширения можно найти в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="14a46-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="14a46-185">**В листинге 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="14a46-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="14a46-186">Расширитель DisabledButton в листинге 2 имеет два свойства с именем TargetButtonID и DisabledText.</span><span class="sxs-lookup"><span data-stu-id="14a46-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="14a46-187">IDReferenceProperty, примененным к свойству TargetButtonID предотвращает назначение ничего, кроме идентификатор элемента управления "Кнопка" для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="14a46-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="14a46-188">Атрибуты WebResource и ClientScriptResource связывание клиентского поведения, в файл с именем DisabledButtonBehavior.js с этого расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="14a46-189">Мы обсудим этот файл JavaScript, в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="14a46-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="14a46-190">Создание пользовательского расширяющего поведение</span><span class="sxs-lookup"><span data-stu-id="14a46-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="14a46-191">Клиентский компонент из управляющего элемента-расширителя называется поведение.</span><span class="sxs-lookup"><span data-stu-id="14a46-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="14a46-192">В поведении DisabledButton содержится фактическая логика для отключения и включения кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="14a46-193">Код JavaScript для поведения включен в листинге 3.</span><span class="sxs-lookup"><span data-stu-id="14a46-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="14a46-194">**Листинг 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="14a46-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="14a46-195">Файл JavaScript в листинге 3 содержит клиентский класс с именем DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="14a46-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="14a46-196">Этот класс, например близнецом на стороне сервера включает в себя два свойства с именем TargetButtonID и получите DisabledText, к которому можно получить с помощью\_TargetButtonID/set\_TargetButtonID и получите\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="14a46-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="14a46-197">Метод initialize() связывает обработчик событий keyup с целевым элементом для поведения.</span><span class="sxs-lookup"><span data-stu-id="14a46-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="14a46-198">Каждый раз, введите букву, в текстовое поле, связанного с этим поведением выполнением обработчика keyup.</span><span class="sxs-lookup"><span data-stu-id="14a46-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="14a46-199">Обработчик keyup включает или отключает кнопку в зависимости от того, содержит ли текстовое поле, связанных с этим поведением любой текст.</span><span class="sxs-lookup"><span data-stu-id="14a46-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="14a46-200">Помните, что необходимо скомпилировать файл JavaScript в листинге 3 как внедренный ресурс.</span><span class="sxs-lookup"><span data-stu-id="14a46-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="14a46-201">В окне обозревателя решений выберите файл, откройте окно свойств и назначьте *внедренный ресурс* для **действие при построении** свойство (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="14a46-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="14a46-202">Этот параметр доступен в Visual Studio и Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="14a46-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="14a46-203">[![Добавление файла JavaScript в качестве внедренного ресурса](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="14a46-204">**Рис 03**: Добавление файла JavaScript как внедренный ресурс ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="14a46-205">Создание пользовательского расширяющего конструктора</span><span class="sxs-lookup"><span data-stu-id="14a46-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="14a46-206">Есть один последний класс, который нам необходимо создать для выполнения наших расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="14a46-207">Нам нужно создать класс конструктора в листинге 4.</span><span class="sxs-lookup"><span data-stu-id="14a46-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="14a46-208">Этот класс обязательно расширителя правильно работать с Visual Studio или Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="14a46-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="14a46-209">**Листинг 4 - DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="14a46-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="14a46-210">Конструктор в листинге 4 связать с DisabledButton расширитель с помощью атрибута конструктора. Необходимо применить атрибут конструктор в класс DisabledButtonExtender следующим образом:</span><span class="sxs-lookup"><span data-stu-id="14a46-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="14a46-211">С помощью пользовательского расширителя</span><span class="sxs-lookup"><span data-stu-id="14a46-211">Using the Custom Extender</span></span>

<span data-ttu-id="14a46-212">Теперь, когда мы завершения создания DisabledButton управляющего элемента-расширителя, настала пора использовать ее в нашем веб-сайте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14a46-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="14a46-213">Во-первых нам нужно добавить на панель инструментов пользовательского расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="14a46-214">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="14a46-214">Follow these steps:</span></span>

1. <span data-ttu-id="14a46-215">Откройте страницу ASP.NET, щелкнув в окне обозревателя решений.</span><span class="sxs-lookup"><span data-stu-id="14a46-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="14a46-216">Щелкните правой кнопкой мыши панель и выберите пункт меню **Выбор элементов**.</span><span class="sxs-lookup"><span data-stu-id="14a46-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="14a46-217">В диалоговом окне Выбор элементов панели элементов перейдите к сборке CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="14a46-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="14a46-218">Нажмите кнопку **ОК** кнопку, чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="14a46-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="14a46-219">После выполнения этих действий DisabledButton управляющего элемента-расширителя появляется в области элементов (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="14a46-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="14a46-220">[![DisabledButton на панели элементов](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="14a46-221">**Рис. 04**: DisabledButton на панели элементов ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="14a46-222">Далее нам нужно создать новую страницу ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14a46-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="14a46-223">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="14a46-223">Follow these steps:</span></span>

1. <span data-ttu-id="14a46-224">Создайте новую страницу ASP.NET с именем ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="14a46-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="14a46-225">ScriptManager перетащите на страницу.</span><span class="sxs-lookup"><span data-stu-id="14a46-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="14a46-226">Перетащите элемент управления TextBox на страницу.</span><span class="sxs-lookup"><span data-stu-id="14a46-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="14a46-227">Перетащите элемент управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="14a46-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="14a46-228">В окне «Свойства» измените значение свойства ИД кнопки <em>btnSave</em> и свойство Text значение *Сохранить\**.</span><span class="sxs-lookup"><span data-stu-id="14a46-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="14a46-229">Мы создали страницу, с помощью стандартного элемента управления ASP.NET TextBox и кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="14a46-230">Далее нам нужно расширить элемент управления TextBox со DisabledButton расширителя:</span><span class="sxs-lookup"><span data-stu-id="14a46-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="14a46-231">Выберите **Добавить расширитель** задач, чтобы открыть диалоговое окно мастера расширений (см. рис. 5).</span><span class="sxs-lookup"><span data-stu-id="14a46-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="14a46-232">Обратите внимание на то, что у диалогового окна есть наш пользовательский расширяющий DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="14a46-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="14a46-233">Выберите расширитель DisabledButton и нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="14a46-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="14a46-234">[![Диалоговое окно мастера расширителя](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="14a46-235">**05 рис**: диалоговое окно мастера расширитель ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="14a46-236">Наконец можно установить свойства расширителя DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="14a46-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="14a46-237">Свойства расширителя DisabledButton можно изменить, изменив свойства элемента управления TextBox:</span><span class="sxs-lookup"><span data-stu-id="14a46-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="14a46-238">Выберите текстовое поле в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="14a46-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="14a46-239">В окне «Свойства» разверните узел расширения (см. рис. 6).</span><span class="sxs-lookup"><span data-stu-id="14a46-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="14a46-240">Назначьте *Сохранить* DisabledText свойство и значение *btnSave* TargetButtonID свойству.</span><span class="sxs-lookup"><span data-stu-id="14a46-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="14a46-241">[![Задание свойств расширителя](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="14a46-242">**Рис 06**: свойства расширителя ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="14a46-243">При запуске страницы (, нажав клавишу F5), изначально отключен элемент управления Button.</span><span class="sxs-lookup"><span data-stu-id="14a46-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="14a46-244">Сразу же начать ввод текста в текстовое поле, кнопку, элемент управления включен (см. рис. 7).</span><span class="sxs-lookup"><span data-stu-id="14a46-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="14a46-245">[![Расширитель DisabledButton в действии](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="14a46-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="14a46-246">**Рис 07**: The DisabledButton расширитель в действии ([Просмотр полноразмерного изображения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="14a46-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="14a46-247">Сводка</span><span class="sxs-lookup"><span data-stu-id="14a46-247">Summary</span></span>

<span data-ttu-id="14a46-248">Цель этого учебника было объяснить, как можно расширить с помощью элементов управления пользовательского расширителя AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="14a46-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="14a46-249">В этом руководстве мы создали простой DisabledButton управляющего элемента-расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="14a46-250">Мы реализовали этого расширителя путем создания класса DisabledButtonExtender, поведение DisabledButtonBehavior JavaScript и класс DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="14a46-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="14a46-251">Выполните аналогичный набор шагов, каждый раз при создании пользовательского элемента управления расширителя.</span><span class="sxs-lookup"><span data-stu-id="14a46-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="14a46-252">[Назад](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Вперед](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="14a46-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
