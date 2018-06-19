---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: С помощью CascadingDropDown с базой данных (C#) | Документы Microsoft
author: wenz
description: CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879131"
---
<a name="using-cascadingdropdown-with-a-database-c"></a><span data-ttu-id="b4938-103">С помощью CascadingDropDown с базой данных (C#)</span><span class="sxs-lookup"><span data-stu-id="b4938-103">Using CascadingDropDown with a Database (C#)</span></span>
====================
<span data-ttu-id="b4938-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b4938-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b4938-105">[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b4938-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span></span>

> <span data-ttu-id="b4938-106">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b4938-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b4938-107">В порядке, чтобы это работало должны создаваться специальные веб-службы.</span><span class="sxs-lookup"><span data-stu-id="b4938-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="b4938-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="b4938-108">Overview</span></span>

<span data-ttu-id="b4938-109">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b4938-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b4938-110">(Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) В порядке, чтобы это работало должны создаваться специальные веб-службы.</span><span class="sxs-lookup"><span data-stu-id="b4938-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="b4938-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="b4938-111">Steps</span></span>

<span data-ttu-id="b4938-112">Во-первых источник данных является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b4938-112">First of all, a data source is required.</span></span> <span data-ttu-id="b4938-113">В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="b4938-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="b4938-114">Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="b4938-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="b4938-115">Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="b4938-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="b4938-116">Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.</span><span class="sxs-lookup"><span data-stu-id="b4938-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="b4938-117">Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b4938-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="b4938-118">Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="b4938-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="b4938-119">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемент):</span><span class="sxs-lookup"><span data-stu-id="b4938-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

<span data-ttu-id="b4938-120">На следующем шаге необходимы два элемента управления DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b4938-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="b4938-121">В этом примере мы используем поставщику и контактной информации из AdventureWorks, таким образом создается один список доступных поставщиков и доступных контактов:</span><span class="sxs-lookup"><span data-stu-id="b4938-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

<span data-ttu-id="b4938-122">Затем необходимо добавить два расширителей CascadingDropDown на страницу.</span><span class="sxs-lookup"><span data-stu-id="b4938-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="b4938-123">Одно заполняет в первом списке (поставщики), а другой заполняет список второй (контактов).</span><span class="sxs-lookup"><span data-stu-id="b4938-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="b4938-124">Необходимо задать следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="b4938-124">The following attributes must be set:</span></span>

- <span data-ttu-id="b4938-125">`ServicePath`: URL-адрес веб-службы доставки элементов списка</span><span class="sxs-lookup"><span data-stu-id="b4938-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="b4938-126">`ServiceMethod`: Веб-метода доставки элементов списка</span><span class="sxs-lookup"><span data-stu-id="b4938-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="b4938-127">`TargetControlID`: Идентификатор из раскрывающегося списка</span><span class="sxs-lookup"><span data-stu-id="b4938-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="b4938-128">`Category`: Категории сведений, переданных при вызове веб-метода</span><span class="sxs-lookup"><span data-stu-id="b4938-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="b4938-129">`PromptText`: Текст, отображаемый при асинхронной загрузке данных списка с сервера</span><span class="sxs-lookup"><span data-stu-id="b4938-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="b4938-130">`ParentControlID`: (необязательно) родительского раскрывающегося списка, загрузкой триггеры текущего списка</span><span class="sxs-lookup"><span data-stu-id="b4938-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="b4938-131">В зависимости от того, используется язык программирования имя веб-службы в вопросе изменяется, но все значения атрибутов совпадают.</span><span class="sxs-lookup"><span data-stu-id="b4938-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="b4938-132">Вот CascadingDropDown элемент для первого раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="b4938-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

<span data-ttu-id="b4938-133">Расширения элемента управления для второго, необходимо указать `ParentControlID` таким образом, чтобы при выборе записи в триггерах список поставщиков загрузке связанных элементов в списке контактов.</span><span class="sxs-lookup"><span data-stu-id="b4938-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

<span data-ttu-id="b4938-134">Фактические трудозатраты, затем выполняется в веб-службы, который настраивается следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b4938-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="b4938-135">Обратите внимание, что `[ScriptService]` используется атрибут, в противном случае ASP.NET AJAX не удается создать прокси-сервера JavaScript для доступа к веб-методов из кода клиентского скрипта.</span><span class="sxs-lookup"><span data-stu-id="b4938-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

<span data-ttu-id="b4938-136">Подпись веб-методов, вызываемых CascadingDropDown выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b4938-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

<span data-ttu-id="b4938-137">Поэтому возвращаемое значение должно быть массивом типа `CascadingDropDownNameValue` определяется набором средств управления.</span><span class="sxs-lookup"><span data-stu-id="b4938-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="b4938-138">`GetVendors()` Метод довольно просто реализовать: код подключается к базе данных AdventureWorks и запрашивает первые 25 поставщиков.</span><span class="sxs-lookup"><span data-stu-id="b4938-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="b4938-139">Первый параметр в `CascadingDropDownNameValue` конструктор имеет заголовок в списке записи второго его значение (значение атрибута в формате HTML &lt; `option` &gt; элемент).</span><span class="sxs-lookup"><span data-stu-id="b4938-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="b4938-140">Ниже приведен код:</span><span class="sxs-lookup"><span data-stu-id="b4938-140">Here is the code:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

<span data-ttu-id="b4938-141">Получение связанного контактов для поставщика (имя метода: `GetContactsForVendor()`) немного сложнее.</span><span class="sxs-lookup"><span data-stu-id="b4938-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="b4938-142">Во-первых необходимо определить поставщика, который был выбран в первом раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="b4938-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="b4938-143">Определяет набор элементов управления вспомогательный метод для этой задачи: `ParseKnownCategoryValuesString()` возвращает `StringDictionary` элемент, содержащий данные раскрывающегося списка:</span><span class="sxs-lookup"><span data-stu-id="b4938-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

<span data-ttu-id="b4938-144">По соображениям безопасности необходимо сначала проверить эти данные.</span><span class="sxs-lookup"><span data-stu-id="b4938-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="b4938-145">Таким образом, если имеется запись поставщика (поскольку `Category` первого элемента CascadingDropDown свойству `"Vendor"`), идентификатор выбранного поставщика может быть получен:</span><span class="sxs-lookup"><span data-stu-id="b4938-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

<span data-ttu-id="b4938-146">Остальная часть метода — довольно прост, затем.</span><span class="sxs-lookup"><span data-stu-id="b4938-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="b4938-147">Идентификатор поставщика используется в качестве параметра для SQL-запрос, который получает все связанные контакты для данного поставщика.</span><span class="sxs-lookup"><span data-stu-id="b4938-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="b4938-148">Опять же, метод возвращает массив объектов типа `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="b4938-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

<span data-ttu-id="b4938-149">Загрузить страницу ASP.NET и через короткое время заполнения списка поставщиков с 25 записей.</span><span class="sxs-lookup"><span data-stu-id="b4938-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="b4938-150">Выберите одну запись и обратите внимание на то, как второй раскрывающийся список заполняется данными.</span><span class="sxs-lookup"><span data-stu-id="b4938-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="b4938-151">[![Первый список заполняется автоматически](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b4938-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span></span>

<span data-ttu-id="b4938-152">Первый список заполняется автоматически ([Просмотр полноразмерное изображение](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b4938-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span></span>


<span data-ttu-id="b4938-153">[![Второй список заполняется согласно выбору, в первом списке](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b4938-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span></span>

<span data-ttu-id="b4938-154">Второй список заполняется согласно выбору, в первом списке ([Просмотр полноразмерное изображение](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b4938-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b4938-155">[Назад](filling-a-list-using-cascadingdropdown-cs.md)
> [Вперед](presetting-list-entries-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b4938-155">[Previous](filling-a-list-using-cascadingdropdown-cs.md)
[Next](presetting-list-entries-with-cascadingdropdown-cs.md)</span></span>
