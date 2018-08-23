---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Начало работы с AJAX Control Toolkit (Visual Basic) | Документация Майкрософт
author: microsoft
description: Узнайте все что нужно знать, чтобы приступить к работе с AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: bbf90d65a0be0eeb4150609aca9cf192f516abf3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829434"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a><span data-ttu-id="d2417-103">Начало работы с AJAX Control Toolkit (VB)</span><span class="sxs-lookup"><span data-stu-id="d2417-103">Get Started with the AJAX Control Toolkit (VB)</span></span>
====================
<span data-ttu-id="d2417-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d2417-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d2417-105">Узнайте все что нужно знать, чтобы приступить к работе с AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="d2417-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="d2417-106">AJAX Control Toolkit содержит более 30 бесплатные элементы управления, которые можно использовать в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d2417-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="d2417-107">В этом руководстве вы узнаете, как скачать AJAX Control Toolkit и добавьте в набор средств элементы управления панели инструментов Visual Studio или Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="d2417-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="d2417-108">Загрузка AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="d2417-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="d2417-109">[AJAX Control Toolkit](http://devexpress.com/act) — это проект с открытым исходным кодом, разработанная компанией членами сообщества ASP.NET и группа разработчиков ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d2417-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span>


<span data-ttu-id="d2417-110">[![Загрузка AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d2417-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span></span>

<span data-ttu-id="d2417-111">**Рис 01**: загрузка AJAX Control Toolkit ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d2417-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span></span>


<span data-ttu-id="d2417-112">После загрузки файла, необходимо разблокировать файл.</span><span class="sxs-lookup"><span data-stu-id="d2417-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="d2417-113">Щелкните правой кнопкой мыши файл, выберите свойства и нажмите кнопку **Unblock** кнопку (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="d2417-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="d2417-114">[![Разблокировка файла ZIP-набор средств управления AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d2417-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span></span>

<span data-ttu-id="d2417-115">**Рис. 02**: разблокирования файла ZIP-набор средств управления AJAX ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="d2417-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span></span>


<span data-ttu-id="d2417-116">После разблокирования файла можно было распаковать файл: щелкните правой кнопкой мыши файл и выберите **извлечь все** пункт меню.</span><span class="sxs-lookup"><span data-stu-id="d2417-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="d2417-117">Теперь мы готовы для добавления в набор средств для панели элементов Visual Studio или Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="d2417-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="d2417-118">Добавление на панель инструментов AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="d2417-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="d2417-119">— Это самый простой способ использования AJAX Control Toolkit, добавляемый в набор средств панели инструментов Visual Studio или Visual Web Developer (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="d2417-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="d2417-120">Таким образом, можно просто перетащить элемент управления набора средств на страницу, если вы хотите использовать его.</span><span class="sxs-lookup"><span data-stu-id="d2417-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="d2417-121">[![AJAX Control Toolkit отображается в панели элементов](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d2417-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span></span>

<span data-ttu-id="d2417-122">**Рис 03**: AJAX Control Toolkit отображается в панели элементов ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d2417-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span></span>


<span data-ttu-id="d2417-123">Во-первых необходимо добавить на панель инструментов с вкладкой AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="d2417-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="d2417-124">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="d2417-124">Follow these steps.</span></span>

1. <span data-ttu-id="d2417-125">Создайте новый веб-сайт ASP.NET, выбрав пункт меню файл, создать веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="d2417-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="d2417-126">Дважды щелкните файл Default.aspx в окне обозревателя решений, чтобы открыть файл в редакторе.</span><span class="sxs-lookup"><span data-stu-id="d2417-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="d2417-127">Щелкните правой кнопкой мыши панель под вкладка "Общие" и выберите пункт меню **добавить вкладку** (см. рис. 4).</span><span class="sxs-lookup"><span data-stu-id="d2417-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="d2417-128">Введите новую вкладку, называемую AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="d2417-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="d2417-129">[![Добавить новую вкладку](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d2417-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span></span>

<span data-ttu-id="d2417-130">**Рис. 04**: добавить новую вкладку ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="d2417-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span></span>


<span data-ttu-id="d2417-131">Далее необходимо добавить элементы управления AJAX Control Toolkit на новой вкладке. Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="d2417-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="d2417-132">Щелкните правой кнопкой мыши под вкладке AJAX Control Toolkit и выбрать пункт меню **Выбор элементов (см. рис. 5)**.</span><span class="sxs-lookup"><span data-stu-id="d2417-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="d2417-133">Перейдите в расположение, где вы распаковали AJAX Control Toolkit и выберите сборку файл AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="d2417-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="d2417-134">[![Выберите элементы для добавления на панель элементов](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d2417-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span></span>

<span data-ttu-id="d2417-135">**05 рис**: Выбор элементов для добавления на панель инструментов ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="d2417-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span></span>


<span data-ttu-id="d2417-136">После выполнения этих действий все элементы набора средств управления будут отображаться в вашем инструментарии.</span><span class="sxs-lookup"><span data-stu-id="d2417-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="d2417-137">Обновление до новой версии набора средств</span><span class="sxs-lookup"><span data-stu-id="d2417-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="d2417-138">Если вы использовали более ранняя версия набора средств и теперь нужно переместить в более поздней версии приведены рекомендуемые действия:</span><span class="sxs-lookup"><span data-stu-id="d2417-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="d2417-139">Двоичные файлы - удалить старую версию файл AjaxControlToolkit.dll сборки из папки Bin веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="d2417-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="d2417-140">Элементы панели элементов - удалить вкладку AJAX Control Toolkit и выполните действия, чтобы повторно создать на вкладке с новой версией сборки, файл AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="d2417-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d2417-141">[Назад](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Вперед](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d2417-141">[Previous](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Next](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span></span>
