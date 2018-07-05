---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Использование CascadingDropDown с базой данных (C#) | Документация Майкрософт
author: wenz
description: Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 06ba008740da7e9cb6a058465154a38b65ccb39a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381620"
---
<a name="using-cascadingdropdown-with-a-database-c"></a><span data-ttu-id="e1adb-103">Использование CascadingDropDown с базой данных (C#)</span><span class="sxs-lookup"><span data-stu-id="e1adb-103">Using CascadingDropDown with a Database (C#)</span></span>
====================
<span data-ttu-id="e1adb-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e1adb-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e1adb-105">[Скачать код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e1adb-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span></span>

> <span data-ttu-id="e1adb-106">Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e1adb-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e1adb-107">В порядке, чтобы это работало должны создаваться специальных веб-службы.</span><span class="sxs-lookup"><span data-stu-id="e1adb-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="e1adb-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="e1adb-108">Overview</span></span>

<span data-ttu-id="e1adb-109">Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e1adb-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e1adb-110">(К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) В порядке, чтобы это работало должны создаваться специальных веб-службы.</span><span class="sxs-lookup"><span data-stu-id="e1adb-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="e1adb-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="e1adb-111">Steps</span></span>

<span data-ttu-id="e1adb-112">Во-первых источником данных является обязательным.</span><span class="sxs-lookup"><span data-stu-id="e1adb-112">First of all, a data source is required.</span></span> <span data-ttu-id="e1adb-113">В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="e1adb-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="e1adb-114">Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="e1adb-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="e1adb-115">Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="e1adb-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="e1adb-116">Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1adb-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="e1adb-117">В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1adb-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="e1adb-118">Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1adb-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="e1adb-119">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемента):</span><span class="sxs-lookup"><span data-stu-id="e1adb-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

<span data-ttu-id="e1adb-120">На следующем шаге необходимы два элемента управления DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e1adb-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="e1adb-121">В этом примере мы используем поставщику и контактной информации из AdventureWorks, таким образом мы создадим один список доступных поставщиков и доступных контактов:</span><span class="sxs-lookup"><span data-stu-id="e1adb-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

<span data-ttu-id="e1adb-122">Затем необходимо добавить два расширителей CascadingDropDown на страницу.</span><span class="sxs-lookup"><span data-stu-id="e1adb-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="e1adb-123">Одно заполняет первый список (поставщики), а другой заполняет второй список (контакты).</span><span class="sxs-lookup"><span data-stu-id="e1adb-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="e1adb-124">Необходимо задать следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="e1adb-124">The following attributes must be set:</span></span>

- <span data-ttu-id="e1adb-125">`ServicePath`: URL-адрес веб-службы, предоставляя элементов списка</span><span class="sxs-lookup"><span data-stu-id="e1adb-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="e1adb-126">`ServiceMethod`: Метод веб-доставка элементов списка</span><span class="sxs-lookup"><span data-stu-id="e1adb-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="e1adb-127">`TargetControlID`: Идентификатор из раскрывающегося списка</span><span class="sxs-lookup"><span data-stu-id="e1adb-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="e1adb-128">`Category`: Категории сведений, переданных при вызове веб-метода</span><span class="sxs-lookup"><span data-stu-id="e1adb-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="e1adb-129">`PromptText`: Текст, отображаемый при асинхронной загрузкой данных с сервера</span><span class="sxs-lookup"><span data-stu-id="e1adb-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="e1adb-130">`ParentControlID`: (необязательно) родительского раскрывающемся списке, что загрузка триггеры текущего списка</span><span class="sxs-lookup"><span data-stu-id="e1adb-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="e1adb-131">В зависимости от используемого языка программирования изменяет имя веб-службы в вопросе, но все остальные значения атрибута совпадают.</span><span class="sxs-lookup"><span data-stu-id="e1adb-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="e1adb-132">Ниже приведен элемент CascadingDropDown для первого раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="e1adb-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

<span data-ttu-id="e1adb-133">Нужно задать управляющих элементов-расширителей для второй список `ParentControlID` таким образом, выберите соответствующую запись в триггерах списка поставщиков загрузке связанных элементов в списке контактов.</span><span class="sxs-lookup"><span data-stu-id="e1adb-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

<span data-ttu-id="e1adb-134">Фактическая работа осуществляется в веб-службы, который настраивается следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e1adb-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="e1adb-135">Обратите внимание, что `[ScriptService]` используется атрибут, в противном случае AJAX для ASP.NET не удается создать прокси JavaScript для доступа к веб-методы из кода клиентского сценария.</span><span class="sxs-lookup"><span data-stu-id="e1adb-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

<span data-ttu-id="e1adb-136">Подпись веб-методов, вызванных CascadingDropDown выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e1adb-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

<span data-ttu-id="e1adb-137">Поэтому возвращаемое значение должно быть массивом типа `CascadingDropDownNameValue` которого определяется набор элементов управления.</span><span class="sxs-lookup"><span data-stu-id="e1adb-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="e1adb-138">`GetVendors()` Метод довольно прост в реализации: код подключается к базе данных AdventureWorks и запрашивает поставщиков первые 25.</span><span class="sxs-lookup"><span data-stu-id="e1adb-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="e1adb-139">Первый параметр в `CascadingDropDownNameValue` конструктор будет ее величина заголовок элемента списка, второй (значение атрибута в формате HTML &lt; `option` &gt; элемент).</span><span class="sxs-lookup"><span data-stu-id="e1adb-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="e1adb-140">Ниже приведен код:</span><span class="sxs-lookup"><span data-stu-id="e1adb-140">Here is the code:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

<span data-ttu-id="e1adb-141">Получение связанных контактов для поставщика (имя метода: `GetContactsForVendor()`) немного сложнее.</span><span class="sxs-lookup"><span data-stu-id="e1adb-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="e1adb-142">Во-первых необходимо определить поставщика, который был выбран в первом раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="e1adb-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="e1adb-143">Набор средств управления определяет вспомогательный метод для выполнения этой задачи: `ParseKnownCategoryValuesString()` возвращает `StringDictionary` элемент, содержащий данные раскрывающегося списка:</span><span class="sxs-lookup"><span data-stu-id="e1adb-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

<span data-ttu-id="e1adb-144">По соображениям безопасности необходимо сначала проверить эти данные.</span><span class="sxs-lookup"><span data-stu-id="e1adb-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="e1adb-145">Таким образом, если имеется запись поставщика (поскольку `Category` первого элемента CascadingDropDown свойству `"Vendor"`), можно извлечь идентификатор выбранного поставщика:</span><span class="sxs-lookup"><span data-stu-id="e1adb-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

<span data-ttu-id="e1adb-146">Остальная часть метода — довольно очевидно, затем.</span><span class="sxs-lookup"><span data-stu-id="e1adb-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="e1adb-147">Идентификатор поставщика используется в качестве параметра для SQL-запрос, который извлекает все связанные контакты для этого поставщика.</span><span class="sxs-lookup"><span data-stu-id="e1adb-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="e1adb-148">Опять же, метод возвращает массив объектов типа `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="e1adb-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

<span data-ttu-id="e1adb-149">Загрузки страницы ASP.NET, и через некоторое время короткий список поставщиков заполняется 25 записей.</span><span class="sxs-lookup"><span data-stu-id="e1adb-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="e1adb-150">Выберите одну запись и обратите внимание на то, каким образом второго раскрывающегося списка заполняется данными.</span><span class="sxs-lookup"><span data-stu-id="e1adb-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="e1adb-151">[![Первый список заполняется автоматически](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e1adb-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span></span>

<span data-ttu-id="e1adb-152">Первый список заполняется автоматически ([Просмотр полноразмерного изображения](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e1adb-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span></span>


<span data-ttu-id="e1adb-153">[![Второй список заполняется согласно выбору, в первом списке](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e1adb-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span></span>

<span data-ttu-id="e1adb-154">Второй список заполняется согласно выбору, в первом списке ([Просмотр полноразмерного изображения](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e1adb-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1adb-155">[Назад](filling-a-list-using-cascadingdropdown-cs.md)
> [Вперед](presetting-list-entries-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e1adb-155">[Previous](filling-a-list-using-cascadingdropdown-cs.md)
[Next](presetting-list-entries-with-cascadingdropdown-cs.md)</span></span>
