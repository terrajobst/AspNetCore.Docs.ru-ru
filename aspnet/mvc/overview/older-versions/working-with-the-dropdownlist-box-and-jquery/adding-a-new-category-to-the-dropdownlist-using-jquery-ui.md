---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Добавление новой категории в DropDownList с помощью пользовательский Интерфейс jQuery | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802530"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="9c72e-102">Добавление новой категории в DropDownList с помощью пользовательского интерфейса jQuery</span><span class="sxs-lookup"><span data-stu-id="9c72e-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="9c72e-103">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9c72e-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="9c72e-104">HTML-код `Select` тег идеально подходит для представления основных категории данных, но зачастую необходимо добавить новую категорию.</span><span class="sxs-lookup"><span data-stu-id="9c72e-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="9c72e-105">Предположим, что мы хотим добавить жанр «Opera» в категории в нашей базе данных?</span><span class="sxs-lookup"><span data-stu-id="9c72e-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="9c72e-106">В этом разделе мы будем использовать пользовательский Интерфейс jQuery, чтобы добавить диалоговое окно, можно использовать для добавления новой категории.</span><span class="sxs-lookup"><span data-stu-id="9c72e-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="9c72e-107">На следующем рисунке показано, как пользовательский Интерфейс будет представлять в браузере.</span><span class="sxs-lookup"><span data-stu-id="9c72e-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="9c72e-108">Когда пользователь выбирает **добавить новый жанр** ссылку, всплывающее диалоговое окно запрашивает у пользователя имя жанр (и, при необходимости, описание).</span><span class="sxs-lookup"><span data-stu-id="9c72e-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="9c72e-109">Ниже показано изображение **добавить жанр** всплывающее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="9c72e-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="9c72e-110">Если введено новое имя жанр и **Сохранить** помещается кнопки, происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="9c72e-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="9c72e-111">Вызов AJAX публикует данные для метода Create объекта контроллера жанра, который сохраняет новый жанр в базе данных и возвращает новый жанр сведения (имя жанр и идентификатора) как JSON.</span><span class="sxs-lookup"><span data-stu-id="9c72e-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="9c72e-112">JavaScript добавляют новые данные жанр к списку выбора.</span><span class="sxs-lookup"><span data-stu-id="9c72e-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="9c72e-113">JavaScript делает новый жанр, выбранный элемент.</span><span class="sxs-lookup"><span data-stu-id="9c72e-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="9c72e-114">На рисунке ниже **Opera** был добавлен в базу данных и выбранного в **жанр** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="9c72e-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="9c72e-115">Откройте *Views\StoreManager\Create.cshtml* файл и замените разметку жанр следующим следующий код:</span><span class="sxs-lookup"><span data-stu-id="9c72e-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="9c72e-116">`_ChooseGenre` Частичное представление будет содержать всю необходимую логику для подключения JavaScript и jQuery, используемый для реализации новой функции жанр add.</span><span class="sxs-lookup"><span data-stu-id="9c72e-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="9c72e-117">Когда мы завершили код, он будет легко сделать то же самое с исполнителя пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9c72e-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="9c72e-118">В обозревателе решений щелкните правой кнопкой мыши *Views\StoreManager* папку и выберите **добавить**, затем **представление**.</span><span class="sxs-lookup"><span data-stu-id="9c72e-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="9c72e-119">В **Имя_представления** ввода, введите `_ChooseGenre` выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="9c72e-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="9c72e-120">Замените существующую разметку в *Views\StoreManager\\_ChooseGenre.cshtml* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="9c72e-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="9c72e-121">В первой строке объявляется, что мы передаем `Album` как модель, точно так же моделировать инструкции, обнаруженной в представление создания.</span><span class="sxs-lookup"><span data-stu-id="9c72e-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="9c72e-122">Следующие несколько строк, **метка** разметке вспомогательной функции.</span><span class="sxs-lookup"><span data-stu-id="9c72e-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="9c72e-123">Следующая строка представляет **DropDownList** вызвать вспомогательный, точно так же, как показано исходное представление создания.</span><span class="sxs-lookup"><span data-stu-id="9c72e-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="9c72e-124">Следующая строка добавляет ссылку с именем `Add New Genre`, и стили его как кнопка.</span><span class="sxs-lookup"><span data-stu-id="9c72e-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="9c72e-125">Строку, содержащую `ValidationMessageFor` копируется непосредственно из представления создания.</span><span class="sxs-lookup"><span data-stu-id="9c72e-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="9c72e-126">Следующие строки:</span><span class="sxs-lookup"><span data-stu-id="9c72e-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="9c72e-127">Создает скрытый элемент div, с Идентификатором `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="9c72e-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="9c72e-128">Мы будем использовать jQuery для подключения наших **добавить жанр** диалоговое окно с Идентификатором `genreDialog` в этот элемент div.</span><span class="sxs-lookup"><span data-stu-id="9c72e-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="9c72e-129">Последние два теги сценариев содержат ссылки на файлы JavaScript, которые будут использоваться для реализации функции добавить новый жанра.</span><span class="sxs-lookup"><span data-stu-id="9c72e-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="9c72e-130">*/Scripts/chooseGenre.js* файл предоставляется для вас в проекте, мы рассмотрим его позже в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="9c72e-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="9c72e-131">Запустите приложение и щелкнуть **добавить новый жанр** кнопки.</span><span class="sxs-lookup"><span data-stu-id="9c72e-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="9c72e-132">В **добавить жанр** диалогового окна введите **Opera** в **имя** поле ввода.</span><span class="sxs-lookup"><span data-stu-id="9c72e-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="9c72e-133">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="9c72e-133">Click the **Save** button.</span></span> <span data-ttu-id="9c72e-134">Вызов AJAX создается категория Opera и затем заполняет раскрывающийся список с Opera и задает Opera как выбранный жанр.</span><span class="sxs-lookup"><span data-stu-id="9c72e-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="9c72e-135">Введите исполнителя, название и цену, а затем выберите **создать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="9c72e-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="9c72e-136">Если ввести цену, меньше $8.99, нового альбома будет отображаться в верхней части представления индекса.</span><span class="sxs-lookup"><span data-stu-id="9c72e-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="9c72e-137">Убедитесь, что новая запись альбома был сохранен в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c72e-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="9c72e-138">Попробуйте создать новый жанр только одной буквы.</span><span class="sxs-lookup"><span data-stu-id="9c72e-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="9c72e-139">В следующем коде в *Models\Genre.cs* файл задает минимальную и максимальную длину имени жанра.</span><span class="sxs-lookup"><span data-stu-id="9c72e-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="9c72e-140">Проверка на стороне клиента сообщает, что необходимо введите строку длиной от 2 до 20 символов.</span><span class="sxs-lookup"><span data-stu-id="9c72e-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="9c72e-141">Изучение как новый жанр добавляется в базу данных и поле со списком.</span><span class="sxs-lookup"><span data-stu-id="9c72e-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="9c72e-142">Откройте *Scripts\chooseGenre.js* файл и проанализировать код.</span><span class="sxs-lookup"><span data-stu-id="9c72e-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="9c72e-143">Вторая строка использует идентификатор `genreDialog` для создания диалогового окна в теге div в *Views\StoreManager\\_ChooseGenre.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="9c72e-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="9c72e-144">Большинство именованных параметров говорят сами за себя.</span><span class="sxs-lookup"><span data-stu-id="9c72e-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="9c72e-145">`autoOpen` Параметр имеет значение false, выбрав **создать жанр** кнопки откроется диалоговое окно явно (описан в последнем).</span><span class="sxs-lookup"><span data-stu-id="9c72e-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="9c72e-146">В диалоговом окне есть две кнопки **Сохранить** и **отменить**.</span><span class="sxs-lookup"><span data-stu-id="9c72e-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="9c72e-147">**Отменить** кнопки закрывает диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="9c72e-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="9c72e-148">В следующем коде показан **Сохранить** кнопку функции.</span><span class="sxs-lookup"><span data-stu-id="9c72e-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="9c72e-149">`var createGenreForm` Нажимаете `createGenreForm` идентификатор.</span><span class="sxs-lookup"><span data-stu-id="9c72e-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="9c72e-150">`createGenreForm` Идентификатор был задан в следующем коде в *Views\Genre\\_CreateGenre.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="9c72e-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="9c72e-151">[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) вспомогательный перегруженный метод, используемый в *Views\Genre\\_CreateGenre.cshtml* файл создает HTML-код с атрибутом действие, содержащее URL-адрес для отправки формы.</span><span class="sxs-lookup"><span data-stu-id="9c72e-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="9c72e-152">Это можно увидеть, отображается страница альбома "Создать" в браузере и выбрав show источника в браузере.</span><span class="sxs-lookup"><span data-stu-id="9c72e-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="9c72e-153">В следующей разметке показан созданный код HTML, содержащий тег формы.</span><span class="sxs-lookup"><span data-stu-id="9c72e-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="9c72e-154">JQuery `$.post` строка делает вызов AJAX в атрибут действия (`/StoreManager/Create`) и передает данные из **создать жанр** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="9c72e-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="9c72e-155">Данные состоит из имени для нового жанр и необязательное описание.</span><span class="sxs-lookup"><span data-stu-id="9c72e-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="9c72e-156">При успешном выполнении вызов AJAX новое имя жанр и значение добавляются к разметке выберите и новый жанр будет присвоено выбранное значение.</span><span class="sxs-lookup"><span data-stu-id="9c72e-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="9c72e-157">Так как это динамически созданной разметки, выберите новый параметр не отображается, просматривая источник в обозревателе.</span><span class="sxs-lookup"><span data-stu-id="9c72e-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="9c72e-158">Вы увидите новый HTML-код с помощью средств разработчика IE 9 F12.</span><span class="sxs-lookup"><span data-stu-id="9c72e-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="9c72e-159">Чтобы просмотреть новый выберите параметр, в Internet Explorer 9, нажмите клавишу F12 для запуска в средствах разработчика F12.</span><span class="sxs-lookup"><span data-stu-id="9c72e-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="9c72e-160">Перейдите на страницу создания и добавьте новый жанр фильма, чтобы новый жанр выбран в списке выбора жанра.</span><span class="sxs-lookup"><span data-stu-id="9c72e-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="9c72e-161">В средствах разработчика F12:</span><span class="sxs-lookup"><span data-stu-id="9c72e-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="9c72e-162">Перейдите на вкладку HTML.</span><span class="sxs-lookup"><span data-stu-id="9c72e-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="9c72e-163">Нажмите значок обновления.</span><span class="sxs-lookup"><span data-stu-id="9c72e-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="9c72e-164">В поле поиска введите GenreID.</span><span class="sxs-lookup"><span data-stu-id="9c72e-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="9c72e-165">С помощью значок "Далее"</span><span class="sxs-lookup"><span data-stu-id="9c72e-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="9c72e-166">Перейдите к следующим тегом выберите:</span><span class="sxs-lookup"><span data-stu-id="9c72e-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="9c72e-167">Разверните последнее значение параметра.</span><span class="sxs-lookup"><span data-stu-id="9c72e-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="9c72e-168">В следующем коде в *Scripts\chooseGenre.js* демонстрирует, как **добавить новый жанр** кнопку подключается к событие click и как **добавить новый жанр** используется диалоговое окно создан.</span><span class="sxs-lookup"><span data-stu-id="9c72e-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="9c72e-169">В первой строке создается нажмите кнопку функции, подключенный к **добавить новый жанр** кнопки.</span><span class="sxs-lookup"><span data-stu-id="9c72e-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="9c72e-170">Следующая разметка из Views\StoreManager\\_ChooseGenre.cshtml файла показан как **добавить новый жанр** создается кнопка:</span><span class="sxs-lookup"><span data-stu-id="9c72e-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="9c72e-171">Метод load создает и открывает диалоговое окно Добавить жанр и вызывает jQuery `parse` метод проверка клиента выполняется на данные, введенные в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="9c72e-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="9c72e-172">В этом разделе вы узнали, как для создания диалогового окна, который может использоваться для добавления новых данных категории в списке выбора.</span><span class="sxs-lookup"><span data-stu-id="9c72e-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="9c72e-173">Необходимо выполнить ту же процедуру для создания пользовательского интерфейса для добавления новых исполнителя в список выбора исполнителя.</span><span class="sxs-lookup"><span data-stu-id="9c72e-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="9c72e-174">Приведенная в этом учебнике сведения о работе со вспомогательной функцией ASP.NET MVC HTML **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="9c72e-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="9c72e-175">Дополнительные сведения о работе с **DropDownList**, см. раздел Добавление ссылки ниже.</span><span class="sxs-lookup"><span data-stu-id="9c72e-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="9c72e-176">Сообщите нам, если этот учебник была для вас полезной.</span><span class="sxs-lookup"><span data-stu-id="9c72e-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="9c72e-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="9c72e-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="9c72e-178">Дополнительные ссылки</span><span class="sxs-lookup"><span data-stu-id="9c72e-178">Additional References</span></span>

- <span data-ttu-id="9c72e-179">[ASP.NET MVC — каскадные раскрывающиеся списки руководстве](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) по [Раду Энука](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="9c72e-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="9c72e-180">[Выбранная](http://harvesthq.github.com/chosen/) подключаемого модуля JavaScript, который поддерживает множественный выбор и фильтрации.</span><span class="sxs-lookup"><span data-stu-id="9c72e-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="9c72e-181">Авторы</span><span class="sxs-lookup"><span data-stu-id="9c72e-181">Contributors</span></span>

- [<span data-ttu-id="9c72e-182">Раду Энука</span><span class="sxs-lookup"><span data-stu-id="9c72e-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="9c72e-183">Жан Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="9c72e-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="9c72e-184">Брэд Вилсон</span><span class="sxs-lookup"><span data-stu-id="9c72e-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="9c72e-185">Рецензенты</span><span class="sxs-lookup"><span data-stu-id="9c72e-185">Reviewers</span></span>

- <span data-ttu-id="9c72e-186">Жан Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="9c72e-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="9c72e-187">Брэд Вилсон</span><span class="sxs-lookup"><span data-stu-id="9c72e-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="9c72e-188">Эти протоколы Майк</span><span class="sxs-lookup"><span data-stu-id="9c72e-188">Mike Pope</span></span>
- <span data-ttu-id="9c72e-189">Том Дайкстра</span><span class="sxs-lookup"><span data-stu-id="9c72e-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9c72e-190">Назад</span><span class="sxs-lookup"><span data-stu-id="9c72e-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
