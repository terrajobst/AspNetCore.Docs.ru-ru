---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Добавление новой категории с помощью jQuery UI DropDownList | Документы Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871939"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="b1b56-102">Добавление новой категории с помощью jQuery UI DropDownList</span><span class="sxs-lookup"><span data-stu-id="b1b56-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="b1b56-103">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b1b56-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="b1b56-104">HTML `Select` тег идеально подходит для представления списка основных категории данных, но зачастую необходимо добавить новую категорию.</span><span class="sxs-lookup"><span data-stu-id="b1b56-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="b1b56-105">Предположим, что мы хотим добавить жанр «Opera» в категории в базе данных?</span><span class="sxs-lookup"><span data-stu-id="b1b56-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="b1b56-106">В этом разделе мы будем использовать jQuery UI, чтобы добавить диалоговое окно, можно использовать для добавления новой категории.</span><span class="sxs-lookup"><span data-stu-id="b1b56-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="b1b56-107">На следующем рисунке показано, как пользовательский Интерфейс будет представлять в браузере.</span><span class="sxs-lookup"><span data-stu-id="b1b56-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="b1b56-108">Когда пользователь выбирает **Добавление новой Genre** ссылку всплывающее диалоговое окно запрашивает имя новой genre (и, Дополнительно, описание).</span><span class="sxs-lookup"><span data-stu-id="b1b56-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="b1b56-109">Ниже показано изображение **добавить жанр** всплывающем диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="b1b56-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="b1b56-110">Если введено новое имя жанр и **Сохранить** помещается кнопка, происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="b1b56-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="b1b56-111">Вызов AJAX отправляет данные для метода Create контроллера жанра, сохраняющий новой genre в базу данных и возвращает новый жанр сведения (имя жанр и идентификатор) как JSON.</span><span class="sxs-lookup"><span data-stu-id="b1b56-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="b1b56-112">JavaScript добавляет новые данные жанр к списку выбора.</span><span class="sxs-lookup"><span data-stu-id="b1b56-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="b1b56-113">JavaScript позволяет новой genre выбранного элемента.</span><span class="sxs-lookup"><span data-stu-id="b1b56-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="b1b56-114">На рисунке ниже **Opera** была добавлена в базу данных и выбрать в **жанр** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="b1b56-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="b1b56-115">Откройте *Views\StoreManager\Create.cshtml* и замените разметку жанр следующие следующий код:</span><span class="sxs-lookup"><span data-stu-id="b1b56-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="b1b56-116">`_ChooseGenre` Частичное представление будет содержать всю логику подключить JavaScript и jQuery используется для реализации добавить новый компонент жанра.</span><span class="sxs-lookup"><span data-stu-id="b1b56-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="b1b56-117">После завершения код будет простой сделать то же самое с исполнителя пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b1b56-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="b1b56-118">В обозревателе решений щелкните правой кнопкой мыши *Views\StoreManager* папку и выберите **добавить**, затем **представление**.</span><span class="sxs-lookup"><span data-stu-id="b1b56-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="b1b56-119">В **имя представления** ввода, введите `_ChooseGenre` выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="b1b56-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="b1b56-120">Замените существующую разметку в *Views\StoreManager\\_ChooseGenre.cshtml* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="b1b56-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="b1b56-121">Первая строка объявляет, что мы передаем `Album` как нашей модели точно так же модели в представлении создать инструкцию.</span><span class="sxs-lookup"><span data-stu-id="b1b56-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="b1b56-122">Следующие несколько строк, **метка** вспомогательный разметки.</span><span class="sxs-lookup"><span data-stu-id="b1b56-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="b1b56-123">Следующая строка представляет **DropDownList** вызвать вспомогательный, точно так же, как и исходное представление создания.</span><span class="sxs-lookup"><span data-stu-id="b1b56-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="b1b56-124">Следующая строка добавляет ссылку с именем `Add New Genre`, и стили его как кнопка.</span><span class="sxs-lookup"><span data-stu-id="b1b56-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="b1b56-125">В строке, содержащей `ValidationMessageFor` копироваться непосредственно из представления создания.</span><span class="sxs-lookup"><span data-stu-id="b1b56-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="b1b56-126">Следующие строки:</span><span class="sxs-lookup"><span data-stu-id="b1b56-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="b1b56-127">Создает скрытый div с Идентификатором `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="b1b56-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="b1b56-128">Мы будем использовать jQuery подключить нашей **добавить жанр** диалоговое окно с Идентификатором `genreDialog` в этом div.</span><span class="sxs-lookup"><span data-stu-id="b1b56-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="b1b56-129">Последние два тегов скрипта содержат ссылки на файлы JavaScript, которые будут использоваться для реализации добавить новый компонент жанра.</span><span class="sxs-lookup"><span data-stu-id="b1b56-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="b1b56-130">*/Scripts/chooseGenre.js* файл предоставляется для вас в проекте будут рассмотрены его позже в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="b1b56-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="b1b56-131">Запустите приложение и щелкнуть **Добавление новой Genre** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b1b56-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="b1b56-132">В **добавить жанр** диалогового окна введите **Opera** в **имя** поля ввода.</span><span class="sxs-lookup"><span data-stu-id="b1b56-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="b1b56-133">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="b1b56-133">Click the **Save** button.</span></span> <span data-ttu-id="b1b56-134">Вызов AJAX создается категория Opera заполняет раскрывающийся список с Opera и задает в качестве выбранных жанр Opera.</span><span class="sxs-lookup"><span data-stu-id="b1b56-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="b1b56-135">Введите имя исполнителя, названия и цены, а затем выберите **создать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b1b56-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="b1b56-136">Если ввести цена меньше $8.99 нового альбома будут отображаться в верхней части представления индекса.</span><span class="sxs-lookup"><span data-stu-id="b1b56-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="b1b56-137">Убедитесь, что новая запись альбом был сохранен в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b1b56-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="b1b56-138">Попробуйте создать новый жанр только одна буква.</span><span class="sxs-lookup"><span data-stu-id="b1b56-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="b1b56-139">В следующем примере кода в *Models\Genre.cs* файл задает минимальную и максимальную длину имени жанра.</span><span class="sxs-lookup"><span data-stu-id="b1b56-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="b1b56-140">Клиентская проверка сообщает, что необходимо ввести строку от 2 до 20 символов.</span><span class="sxs-lookup"><span data-stu-id="b1b56-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="b1b56-141">Изучение как новый жанр добавляется в базу данных и поле со списком.</span><span class="sxs-lookup"><span data-stu-id="b1b56-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="b1b56-142">Откройте *Scripts\chooseGenre.js* файла и изучите его код.</span><span class="sxs-lookup"><span data-stu-id="b1b56-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="b1b56-143">Во второй строке используется идентификатор `genreDialog` для создания диалогового окна в тег div в *Views\StoreManager\\_ChooseGenre.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="b1b56-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="b1b56-144">Большинство именованных параметров говорят сами за себя.</span><span class="sxs-lookup"><span data-stu-id="b1b56-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="b1b56-145">`autoOpen` Параметр имеет значение false, при выборе **создать жанр** явно открыть диалоговое окно (это описано в последнем).</span><span class="sxs-lookup"><span data-stu-id="b1b56-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="b1b56-146">В диалоговом окне есть две кнопки **Сохранить** и **отменить**.</span><span class="sxs-lookup"><span data-stu-id="b1b56-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="b1b56-147">**Отменить** кнопка закрытия диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="b1b56-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="b1b56-148">В следующем коде показано **Сохранить** кнопку функции.</span><span class="sxs-lookup"><span data-stu-id="b1b56-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="b1b56-149">`var createGenreForm` Выбирается из `createGenreForm` идентификатор.</span><span class="sxs-lookup"><span data-stu-id="b1b56-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="b1b56-150">`createGenreForm` Идентификатор был задан в следующем коде в *Views\Genre\\_CreateGenre.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="b1b56-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="b1b56-151">[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) вспомогательный перегруженный метод, используемый в *Views\Genre\\_CreateGenre.cshtml* файл создает HTML-код с атрибутом действий, содержащую URL-адрес для отправки формы.</span><span class="sxs-lookup"><span data-stu-id="b1b56-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="b1b56-152">Это можно увидеть, отображение страницы создания альбом в браузере и выбрав источник Показать в браузере.</span><span class="sxs-lookup"><span data-stu-id="b1b56-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="b1b56-153">В следующем примере показана созданного кода HTML, содержащий тег формы.</span><span class="sxs-lookup"><span data-stu-id="b1b56-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="b1b56-154">JQuery `$.post` строки осуществляет вызов AJAX атрибут действия (`/StoreManager/Create`) и передает данные из **создать жанр** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="b1b56-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="b1b56-155">Данных состоит из имени для нового жанр и описание (необязательно).</span><span class="sxs-lookup"><span data-stu-id="b1b56-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="b1b56-156">При успешном выполнении вызов AJAX выберите разметки добавляются новые жанр имя и значение и новой genre присвоено выбранное значение.</span><span class="sxs-lookup"><span data-stu-id="b1b56-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="b1b56-157">Это динамически создаваемых разметки, выберите новый параметр нельзя просмотреть, просматривая разметку в браузере.</span><span class="sxs-lookup"><span data-stu-id="b1b56-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="b1b56-158">Вы увидите новый HTML с помощью средств разработчика IE 9 F12.</span><span class="sxs-lookup"><span data-stu-id="b1b56-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="b1b56-159">Для просмотра выберите новый параметр в Internet Explorer 9, нажмите клавишу F12 для запуска средств разработчика F12.</span><span class="sxs-lookup"><span data-stu-id="b1b56-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="b1b56-160">Перейдите на страницу создания и добавить новый жанра, чтобы новый жанр выбран в списке выбора жанра.</span><span class="sxs-lookup"><span data-stu-id="b1b56-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="b1b56-161">В средствах разработчика F12:</span><span class="sxs-lookup"><span data-stu-id="b1b56-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="b1b56-162">Перейдите на вкладку HTML.</span><span class="sxs-lookup"><span data-stu-id="b1b56-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="b1b56-163">Нажмите значок «Обновить».</span><span class="sxs-lookup"><span data-stu-id="b1b56-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="b1b56-164">В поле поиска введите GenreID.</span><span class="sxs-lookup"><span data-stu-id="b1b56-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="b1b56-165">Используя значок "Далее"</span><span class="sxs-lookup"><span data-stu-id="b1b56-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="b1b56-166">Перейдите к выберите следующий тег:</span><span class="sxs-lookup"><span data-stu-id="b1b56-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="b1b56-167">Разверните последнее значение параметра.</span><span class="sxs-lookup"><span data-stu-id="b1b56-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="b1b56-168">В него следующий код *Scripts\chooseGenre.js* файла показано, как **Добавление новой Genre** кнопку подключается к события щелчка кнопкой мыши и как **Добавление новой Genre** используется диалоговое окно создать.</span><span class="sxs-lookup"><span data-stu-id="b1b56-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="b1b56-169">В первой строке создается щелкните функции, подключенный к **Добавление новой Genre** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b1b56-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="b1b56-170">Приведенная ниже разметка из Views\StoreManager\\_ChooseGenre.cshtml показан файл как **Добавление новой Genre** кнопка для:</span><span class="sxs-lookup"><span data-stu-id="b1b56-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="b1b56-171">Метод load создает и открывает диалоговое окно Добавить жанр и вызывает jQuery `parse` метод, поэтому проверка клиента выполняется на данные, введенные в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="b1b56-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="b1b56-172">В этом разделе вы узнали, как для создания диалогового окна, который может использоваться для добавления новой категории данных в списке выбора.</span><span class="sxs-lookup"><span data-stu-id="b1b56-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="b1b56-173">Необходимо выполнить той же процедуры для создания пользовательского интерфейса для добавления нового исполнителя в список выбора исполнителя.</span><span class="sxs-lookup"><span data-stu-id="b1b56-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="b1b56-174">Этот учебник предоставил сведения о работе с вспомогательный метод ASP.NET MVC HTML **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="b1b56-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="b1b56-175">Дополнительные сведения о работе с **DropDownList**, разделе «Добавление ссылки» ниже.</span><span class="sxs-lookup"><span data-stu-id="b1b56-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="b1b56-176">Сообщите нам, если этого учебника было полезным.</span><span class="sxs-lookup"><span data-stu-id="b1b56-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="b1b56-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="b1b56-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="b1b56-178">Дополнительные ссылки</span><span class="sxs-lookup"><span data-stu-id="b1b56-178">Additional References</span></span>

- <span data-ttu-id="b1b56-179">[ASP.NET MVC — каскадные раскрывающихся списков учебника](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) по [Раду Энука](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="b1b56-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="b1b56-180">[Выбранный](http://harvesthq.github.com/chosen/) подключаемого модуля JavaScript, который поддерживает множественного выбора и фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b1b56-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="b1b56-181">Авторы</span><span class="sxs-lookup"><span data-stu-id="b1b56-181">Contributors</span></span>

- [<span data-ttu-id="b1b56-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="b1b56-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="b1b56-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="b1b56-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="b1b56-184">Брэд Вилсон</span><span class="sxs-lookup"><span data-stu-id="b1b56-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="b1b56-185">Рецензенты</span><span class="sxs-lookup"><span data-stu-id="b1b56-185">Reviewers</span></span>

- <span data-ttu-id="b1b56-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="b1b56-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="b1b56-187">Брэд Вилсон</span><span class="sxs-lookup"><span data-stu-id="b1b56-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="b1b56-188">Эти протоколы Майк</span><span class="sxs-lookup"><span data-stu-id="b1b56-188">Mike Pope</span></span>
- <span data-ttu-id="b1b56-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="b1b56-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b1b56-190">Назад</span><span class="sxs-lookup"><span data-stu-id="b1b56-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
