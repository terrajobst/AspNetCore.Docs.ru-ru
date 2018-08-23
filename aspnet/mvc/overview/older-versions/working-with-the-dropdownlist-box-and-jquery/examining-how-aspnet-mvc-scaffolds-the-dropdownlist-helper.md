---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Проверка того, как ASP.NET MVC формирует шаблоны вспомогательного метода DropDownList | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ab45a14c4eda9e7552af4831851396af3c13dce1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835928"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="df087-102">Проверка того, как ASP.NET MVC формирует шаблоны вспомогательного метода DropDownList</span><span class="sxs-lookup"><span data-stu-id="df087-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="df087-103">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="df087-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="df087-104">В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папку, а затем выберите **Добавление контроллера**.</span><span class="sxs-lookup"><span data-stu-id="df087-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="df087-105">Назовите контроллер **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="df087-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="df087-106">Для установки параметров **Добавление контроллера** диалоговое окно, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="df087-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="df087-107">Изменить *StoreManager\Index.cshtml* Просмотр и удаление `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="df087-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="df087-108">Удаление `AlbumArtUrl` будет сделать презентации более удобочитаемым.</span><span class="sxs-lookup"><span data-stu-id="df087-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="df087-109">Ниже приведен готовый код.</span><span class="sxs-lookup"><span data-stu-id="df087-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="df087-110">Откройте *Controllers\StoreManagerController.cs* файл и найдите `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="df087-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="df087-111">Добавить `OrderBy` предложение, поэтому дисках будут отсортированы по цене.</span><span class="sxs-lookup"><span data-stu-id="df087-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="df087-112">Ниже приведен полный код.</span><span class="sxs-lookup"><span data-stu-id="df087-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="df087-113">Сортировка по цене облегчит его проверить изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="df087-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="df087-114">При тестировании редактирования и создания методов, можно использовать низкая цена, сохраненные данные будут отображаться первыми.</span><span class="sxs-lookup"><span data-stu-id="df087-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="df087-115">Откройте *StoreManager\Edit.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="df087-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="df087-116">Добавьте следующую строку сразу после тега условных обозначений.</span><span class="sxs-lookup"><span data-stu-id="df087-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="df087-117">В следующем коде показано контекст этого изменения:</span><span class="sxs-lookup"><span data-stu-id="df087-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="df087-118">`AlbumId` Требуется вносить изменения в запись альбома.</span><span class="sxs-lookup"><span data-stu-id="df087-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="df087-119">Нажмите CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="df087-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="df087-120">Выберите, чтобы **администратора** ссылку, а затем выберите **Create New** связи для создания нового альбома.</span><span class="sxs-lookup"><span data-stu-id="df087-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="df087-121">Убедитесь, что сохраненные сведения о диске.</span><span class="sxs-lookup"><span data-stu-id="df087-121">Verify the album information was saved.</span></span> <span data-ttu-id="df087-122">Изменить альбома и убедитесь, что внесенные изменения сохраняются.</span><span class="sxs-lookup"><span data-stu-id="df087-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="df087-123">Схема альбома</span><span class="sxs-lookup"><span data-stu-id="df087-123">The Album Schema</span></span>

<span data-ttu-id="df087-124">`StoreManager` Контроллера, созданные с помощью механизма формирования шаблонов MVC разрешает доступ CRUD (Create, Read, Update, Delete) и альбомы, в базе данных магазина музыки.</span><span class="sxs-lookup"><span data-stu-id="df087-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="df087-125">Ниже приводится схема для сведения об альбоме.</span><span class="sxs-lookup"><span data-stu-id="df087-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="df087-126">`Albums` Таблице не хранятся жанр альбома и описание, он сохраняет внешний ключ к `Genres` таблицы.</span><span class="sxs-lookup"><span data-stu-id="df087-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="df087-127">`Genres` Таблица содержит описание и название жанра.</span><span class="sxs-lookup"><span data-stu-id="df087-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="df087-128">Аналогичным образом `Albums` таблица не содержит имя альбома исполнители, но внешний ключ к `Artists` таблицы.</span><span class="sxs-lookup"><span data-stu-id="df087-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="df087-129">`Artists` Таблица содержит имя исполнителя.</span><span class="sxs-lookup"><span data-stu-id="df087-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="df087-130">Если вы изучите данные в `Albums` таблицы, можно увидеть, каждая строка содержит внешний ключ к `Genres` таблицы и внешний ключ к `Artists` таблицы.</span><span class="sxs-lookup"><span data-stu-id="df087-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="df087-131">На следующем рисунке показано некоторые данные таблицы из `Albums` таблицы.</span><span class="sxs-lookup"><span data-stu-id="df087-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="df087-132">Тег HTML Select</span><span class="sxs-lookup"><span data-stu-id="df087-132">The HTML Select Tag</span></span>

<span data-ttu-id="df087-133">HTML-код `<select>` элемент (созданные HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) вспомогательный) используется для отображения полного списка значений (например, список жанров).</span><span class="sxs-lookup"><span data-stu-id="df087-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="df087-134">Для формы редактирования когда известно текущее значение в списке выбора можно отобразить текущее значение.</span><span class="sxs-lookup"><span data-stu-id="df087-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="df087-135">Мы видели ранее данном когда мы присвоено выбранное значение **комедия**.</span><span class="sxs-lookup"><span data-stu-id="df087-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="df087-136">Список выбора идеально подходит для отображения категорий или данные внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="df087-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="df087-137">`<select>` Элемент для внешнего ключа жанр отображает список имен возможных жанра, но обновляется при сохранении формы свойство жанр жанр значения внешнего ключа, не отображаемых жанр имя.</span><span class="sxs-lookup"><span data-stu-id="df087-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="df087-138">В приведенном ниже рисунке является выбранного жанра **Disco** и исполнителя **лето Дарья**.</span><span class="sxs-lookup"><span data-stu-id="df087-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="df087-139">Изучение ASP.NET MVC сформированные кода</span><span class="sxs-lookup"><span data-stu-id="df087-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="df087-140">Откройте *Controllers\StoreManagerController.cs* файл и найдите `HTTP GET Create` метод.</span><span class="sxs-lookup"><span data-stu-id="df087-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="df087-141">`Create` Метод складывает два [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) объектов `ViewBag`, в них сведения жанр и в них сведения исполнителя.</span><span class="sxs-lookup"><span data-stu-id="df087-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="df087-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx) перегрузку конструктора, выше принимает три аргумента:</span><span class="sxs-lookup"><span data-stu-id="df087-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="df087-143">*элементы*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) содержащий элементы в списке.</span><span class="sxs-lookup"><span data-stu-id="df087-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="df087-144">В приведенном выше примере возвращаемый список жанров `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="df087-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="df087-145">*DataValueField используются*: имя свойства в **IEnumerable** список, содержащий значение ключа.</span><span class="sxs-lookup"><span data-stu-id="df087-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="df087-146">В примере выше `GenreId` и `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="df087-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="df087-147">*dataTextField*: имя свойства в **IEnumerable** список, содержащий сведения для отображения.</span><span class="sxs-lookup"><span data-stu-id="df087-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="df087-148">В таблице жанр и исполнители `name` используется поле.</span><span class="sxs-lookup"><span data-stu-id="df087-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="df087-149">Откройте *Views\StoreManager\Create.cshtml* файл и просмотрите `Html.DropDownList` разметке вспомогательной функции для жанра, полю.</span><span class="sxs-lookup"><span data-stu-id="df087-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="df087-150">Первая строка показывает, что требуется создать представление `Album` модели.</span><span class="sxs-lookup"><span data-stu-id="df087-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="df087-151">В `Create` в приведенном выше примере, модель не был передан, поэтому представление получает **null** `Album` модели.</span><span class="sxs-lookup"><span data-stu-id="df087-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="df087-152">На этом этапе мы при создании нового альбома, поэтому мы не установлены `Album` данных для него.</span><span class="sxs-lookup"><span data-stu-id="df087-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="df087-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) перегрузка, показанный выше принимает имя поля для привязки к модели.</span><span class="sxs-lookup"><span data-stu-id="df087-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="df087-154">Это имя также используется для поиска **ViewBag** объект, содержащий [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) объекта.</span><span class="sxs-lookup"><span data-stu-id="df087-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="df087-155">Используя эту перегрузку, не требуется имя **ViewBag SelectList** объект `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="df087-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="df087-156">Второй параметр (`String.Empty`) — это текст, который отображается, если элемент не выбран.</span><span class="sxs-lookup"><span data-stu-id="df087-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="df087-157">Это именно то, что необходимо при создании нового альбома.</span><span class="sxs-lookup"><span data-stu-id="df087-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="df087-158">Если вы удаляли второго параметра и использовал следующий код:</span><span class="sxs-lookup"><span data-stu-id="df087-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="df087-159">Список выбора может по умолчанию первый элемент, то есть рок в нашем примере.</span><span class="sxs-lookup"><span data-stu-id="df087-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="df087-160">Изучение `HTTP POST Create` метод.</span><span class="sxs-lookup"><span data-stu-id="df087-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="df087-161">Эта перегрузка `Create` альбома `album` объект, созданный системой привязки модели ASP.NET MVC из формы значения, переданные.</span><span class="sxs-lookup"><span data-stu-id="df087-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="df087-162">При отправке нового альбома, если состояние модели является допустимым и отсутствуют ошибки базы данных, нового альбома добавляется базы данных.</span><span class="sxs-lookup"><span data-stu-id="df087-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="df087-163">Ниже показано создание нового альбома.</span><span class="sxs-lookup"><span data-stu-id="df087-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="df087-164">Можно использовать [инструмента fiddler](http://www.fiddler2.com/fiddler2/) изучаемый отправленные значения формы, привязку модели ASP.NET MVC использует для создания объекта альбома.</span><span class="sxs-lookup"><span data-stu-id="df087-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="df087-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="df087-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="df087-166">Рефакторинг, создание ViewBag SelectList</span><span class="sxs-lookup"><span data-stu-id="df087-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="df087-167">Оба `Edit` методы и `HTTP POST Create` метод имеют идентичный код для настройки **SelectList** в **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="df087-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="df087-168">Из-за специфики ["не повторяйся"](http://en.wikipedia.org/wiki/Don't_repeat_yourself), мы будет рефакторинг данного кода.</span><span class="sxs-lookup"><span data-stu-id="df087-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="df087-169">Мы сделаем это использование рефакторинг кода более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="df087-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="df087-170">Создайте новый метод для добавления жанр и исполнителях **SelectList** для **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="df087-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="df087-171">Замените две строки, параметр `ViewBag` в каждом из `Create` и `Edit` методы с помощью вызова `SetGenreArtistViewBag` метод.</span><span class="sxs-lookup"><span data-stu-id="df087-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="df087-172">Ниже приведен готовый код.</span><span class="sxs-lookup"><span data-stu-id="df087-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="df087-173">Создание нового альбома и редактирование альбом, чтобы убедиться, что изменения работают.</span><span class="sxs-lookup"><span data-stu-id="df087-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="df087-174">Явно передающей SelectList в элемент управления DropDownList</span><span class="sxs-lookup"><span data-stu-id="df087-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="df087-175">Создание и изменение представления, созданные с помощью формирования шаблонов ASP.NET MVC следующие **DropDownList** перегрузки:</span><span class="sxs-lookup"><span data-stu-id="df087-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="df087-176">`DropDownList` Разметка для представления создания, показано ниже.</span><span class="sxs-lookup"><span data-stu-id="df087-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="df087-177">Так как `ViewBag` свойство для `SelectList` называется `GenreId`, **DropDownList** будет использовать вспомогательные `GenreId` **SelectList** в **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="df087-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="df087-178">В следующем **DropDownList** перегрузки, `SelectList` передается явно в.</span><span class="sxs-lookup"><span data-stu-id="df087-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="df087-179">Откройте *Views\StoreManager\Edit.cshtml* файлов и изменить **DropDownList** вызов явным образом передать **SelectList**, с помощью перегрузки выше.</span><span class="sxs-lookup"><span data-stu-id="df087-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="df087-180">Это необходимо сделайте для категории по жанру.</span><span class="sxs-lookup"><span data-stu-id="df087-180">Do this for the Genre category.</span></span> <span data-ttu-id="df087-181">Ниже приведен полный код:</span><span class="sxs-lookup"><span data-stu-id="df087-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="df087-182">Запустите приложение и нажмите кнопку **администратора** ссылку, а затем перейдите к альбома Jazz и выберите **изменить** ссылку.</span><span class="sxs-lookup"><span data-stu-id="df087-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="df087-183">Вместо отображения Jazz как текущего выбранного жанра, рок отображается.</span><span class="sxs-lookup"><span data-stu-id="df087-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="df087-184">Если строковый аргумент (свойство для привязки) и **SelectList** объекта имеют одинаковое имя, выбранное значение не используется.</span><span class="sxs-lookup"><span data-stu-id="df087-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="df087-185">При отсутствии выбранных значение не указано, по умолчанию браузеры на первый элемент в **SelectList**(который является **рок** в приведенном выше примере).</span><span class="sxs-lookup"><span data-stu-id="df087-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="df087-186">Это известное ограничение **DropDownList** вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="df087-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="df087-187">Откройте *Controllers\StoreManagerController.cs* файлов и изменить **SelectList** имена для объектов `Genres` и `Artists`.</span><span class="sxs-lookup"><span data-stu-id="df087-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="df087-188">Ниже приведен полный код:</span><span class="sxs-lookup"><span data-stu-id="df087-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="df087-189">Имена жанры и исполнители являются более понятные имена для категорий, так как они содержат больше, чем просто идентификатор каждой категории.</span><span class="sxs-lookup"><span data-stu-id="df087-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="df087-190">Рефакторинг, которые мы делали ранее окупилась.</span><span class="sxs-lookup"><span data-stu-id="df087-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="df087-191">Вместо изменения **ViewBag** в четыре метода изменения изолируются в `SetGenreArtistViewBag` метод.</span><span class="sxs-lookup"><span data-stu-id="df087-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="df087-192">Изменение **DropDownList** вызова в создание и изменение представлений для использования нового **SelectList** имена.</span><span class="sxs-lookup"><span data-stu-id="df087-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="df087-193">Ниже показана новая разметка для представления изменения:</span><span class="sxs-lookup"><span data-stu-id="df087-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="df087-194">Создать представление требует пустую строку, чтобы предотвратить отображение первого элемента в SelectList.</span><span class="sxs-lookup"><span data-stu-id="df087-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="df087-195">Создание нового альбома и редактирование альбом, чтобы убедиться, что изменения работают.</span><span class="sxs-lookup"><span data-stu-id="df087-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="df087-196">Тестировать код редактирования, выбрав альбом с жанра, отличный от рок.</span><span class="sxs-lookup"><span data-stu-id="df087-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="df087-197">С помощью модели представления с помощью вспомогательного метода DropDownList</span><span class="sxs-lookup"><span data-stu-id="df087-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="df087-198">Создайте новый класс в папку ViewModels `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="df087-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="df087-199">Замените код в `AlbumSelectListViewModel` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="df087-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="df087-200">`AlbumSelectListViewModel` Конструктор принимает альбом, список исполнителей и жанров и создает объект, содержащий альбома и `SelectList` жанров и исполнителей.</span><span class="sxs-lookup"><span data-stu-id="df087-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="df087-201">Постройте проект таким образом `AlbumSelectListViewModel` доступна при следующем шаге мы создаем общее представление.</span><span class="sxs-lookup"><span data-stu-id="df087-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="df087-202">Добавить `EditVM` метод `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="df087-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="df087-203">Ниже приведен готовый код.</span><span class="sxs-lookup"><span data-stu-id="df087-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="df087-204">Щелкните правой кнопкой мыши `AlbumSelectListViewModel`выберите **устранить**, затем **с помощью MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="df087-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="df087-205">Кроме того, можно добавить следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="df087-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="df087-206">Щелкните правой кнопкой мыши `EditVM` и выберите **Добавление представления**.</span><span class="sxs-lookup"><span data-stu-id="df087-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="df087-207">Используйте параметры, приведенные ниже.</span><span class="sxs-lookup"><span data-stu-id="df087-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="df087-208">Выберите **добавить**, затем замените содержимое файла *Views\StoreManager\EditVM.cshtml* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="df087-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="df087-209">`EditVM` Разметки во многом схожа с исходным `Edit` разметки со следующими исключениями.</span><span class="sxs-lookup"><span data-stu-id="df087-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="df087-210">Свойств в модели `Edit` представления имеют вид `model.property`(например, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="df087-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="df087-211">Свойств в модели `EditVm` представления имеют вид `model.Album.property`(например, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="df087-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="df087-212">Это потому, что `EditVM` представление передается контейнер `Album`, а не `Album` как в `Edit` представления.</span><span class="sxs-lookup"><span data-stu-id="df087-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="df087-213">**DropDownList** второго параметра поступает из модели представления, не **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="df087-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="df087-214">**BeginForm** вспомогательная функция в `EditVM` представления явно обратной передаче `Edit` метода действия.</span><span class="sxs-lookup"><span data-stu-id="df087-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="df087-215">Публикуя обратно в `Edit` действия, мы не нужно писать `HTTP POST EditVM` действия могут использовать `HTTP POST` `Edit` действие.</span><span class="sxs-lookup"><span data-stu-id="df087-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="df087-216">Запустите приложение и изменить альбома.</span><span class="sxs-lookup"><span data-stu-id="df087-216">Run the application and edit an album.</span></span> <span data-ttu-id="df087-217">Изменить URL-адрес, используемый `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="df087-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="df087-218">Изменение поля и нажмите кнопку **Сохранить** кнопку, чтобы убедиться в работоспособности кода.</span><span class="sxs-lookup"><span data-stu-id="df087-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="df087-219">Какой подход следует использовать?</span><span class="sxs-lookup"><span data-stu-id="df087-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="df087-220">Все три подхода показано прерывая запись.</span><span class="sxs-lookup"><span data-stu-id="df087-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="df087-221">Многие разработчики предпочитают explictily pass `SelectList` для `DropDownList` с помощью `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="df087-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="df087-222">Такой подход имеет дополнительное преимущество дает гибкость использования более подходящего имени для коллекции.</span><span class="sxs-lookup"><span data-stu-id="df087-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="df087-223">Единственным условием является нельзя присвоить имя `ViewBag SelectList` объекта совпадает с именем свойства модели.</span><span class="sxs-lookup"><span data-stu-id="df087-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="df087-224">Некоторые разработчики предпочитают подхода ViewModel.</span><span class="sxs-lookup"><span data-stu-id="df087-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="df087-225">Другие рассмотрим более подробный разметку и создания кода HTML подхода ViewModel недостаток.</span><span class="sxs-lookup"><span data-stu-id="df087-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="df087-226">В этом разделе мы узнали три подхода к использованию **DropDownList** с категории данных.</span><span class="sxs-lookup"><span data-stu-id="df087-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="df087-227">В следующем разделе будет показано, как добавить новую категорию.</span><span class="sxs-lookup"><span data-stu-id="df087-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df087-228">[Назад](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Вперед](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="df087-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
