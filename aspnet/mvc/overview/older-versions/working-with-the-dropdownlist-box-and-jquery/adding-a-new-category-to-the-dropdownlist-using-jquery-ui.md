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
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Добавление новой категории с помощью jQuery UI DropDownList
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

HTML `Select` тег идеально подходит для представления списка основных категории данных, но зачастую необходимо добавить новую категорию. Предположим, что мы хотим добавить жанр «Opera» в категории в базе данных? В этом разделе мы будем использовать jQuery UI, чтобы добавить диалоговое окно, можно использовать для добавления новой категории. На следующем рисунке показано, как пользовательский Интерфейс будет представлять в браузере.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Когда пользователь выбирает **Добавление новой Genre** ссылку всплывающее диалоговое окно запрашивает имя новой genre (и, Дополнительно, описание). Ниже показано изображение **добавить жанр** всплывающем диалоговом окне.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Если введено новое имя жанр и **Сохранить** помещается кнопка, происходит следующее:

1. Вызов AJAX отправляет данные для метода Create контроллера жанра, сохраняющий новой genre в базу данных и возвращает новый жанр сведения (имя жанр и идентификатор) как JSON.
2. JavaScript добавляет новые данные жанр к списку выбора.
3. JavaScript позволяет новой genre выбранного элемента.

   На рисунке ниже **Opera** была добавлена в базу данных и выбрать в **жанр** раскрывающегося списка. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Откройте *Views\StoreManager\Create.cshtml* и замените разметку жанр следующие следующий код:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Частичное представление будет содержать всю логику подключить JavaScript и jQuery используется для реализации добавить новый компонент жанра. После завершения код будет простой сделать то же самое с исполнителя пользовательского интерфейса.

В обозревателе решений щелкните правой кнопкой мыши *Views\StoreManager* папку и выберите **добавить**, затем **представление**. В **имя представления** ввода, введите `_ChooseGenre` выберите **добавить**. Замените существующую разметку в *Views\StoreManager\\_ChooseGenre.cshtml* файл со следующим:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Первая строка объявляет, что мы передаем `Album` как нашей модели точно так же модели в представлении создать инструкцию. Следующие несколько строк, **метка** вспомогательный разметки. Следующая строка представляет **DropDownList** вызвать вспомогательный, точно так же, как и исходное представление создания. Следующая строка добавляет ссылку с именем `Add New Genre`, и стили его как кнопка. В строке, содержащей `ValidationMessageFor` копироваться непосредственно из представления создания. Следующие строки:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Создает скрытый div с Идентификатором `genreDialog`. Мы будем использовать jQuery подключить нашей **добавить жанр** диалоговое окно с Идентификатором `genreDialog` в этом div. Последние два тегов скрипта содержат ссылки на файлы JavaScript, которые будут использоваться для реализации добавить новый компонент жанра. */Scripts/chooseGenre.js* файл предоставляется для вас в проекте будут рассмотрены его позже в этом учебнике.

Запустите приложение и щелкнуть **Добавление новой Genre** кнопки. В **добавить жанр** диалогового окна введите **Opera** в **имя** поля ввода.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Нажмите кнопку **Сохранить**. Вызов AJAX создается категория Opera заполняет раскрывающийся список с Opera и задает в качестве выбранных жанр Opera.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Введите имя исполнителя, названия и цены, а затем выберите **создать** кнопки. Если ввести цена меньше $8.99 нового альбома будут отображаться в верхней части представления индекса. Убедитесь, что новая запись альбом был сохранен в базе данных.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Попробуйте создать новый жанр только одна буква. В следующем примере кода в *Models\Genre.cs* файл задает минимальную и максимальную длину имени жанра.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Клиентская проверка сообщает, что необходимо ввести строку от 2 до 20 символов.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Изучение как новый жанр добавляется в базу данных и поле со списком.

Откройте *Scripts\chooseGenre.js* файла и изучите его код.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Во второй строке используется идентификатор `genreDialog` для создания диалогового окна в тег div в *Views\StoreManager\\_ChooseGenre.cshtml* файла. Большинство именованных параметров говорят сами за себя. `autoOpen` Параметр имеет значение false, при выборе **создать жанр** явно открыть диалоговое окно (это описано в последнем). В диалоговом окне есть две кнопки **Сохранить** и **отменить**. **Отменить** кнопка закрытия диалогового окна. В следующем коде показано **Сохранить** кнопку функции.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Выбирается из `createGenreForm` идентификатор. `createGenreForm` Идентификатор был задан в следующем коде в *Views\Genre\\_CreateGenre.cshtml* файла.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) вспомогательный перегруженный метод, используемый в *Views\Genre\\_CreateGenre.cshtml* файл создает HTML-код с атрибутом действий, содержащую URL-адрес для отправки формы. Это можно увидеть, отображение страницы создания альбом в браузере и выбрав источник Показать в браузере. В следующем примере показана созданного кода HTML, содержащий тег формы.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` строки осуществляет вызов AJAX атрибут действия (`/StoreManager/Create`) и передает данные из **создать жанр** диалоговое окно. Данных состоит из имени для нового жанр и описание (необязательно). При успешном выполнении вызов AJAX выберите разметки добавляются новые жанр имя и значение и новой genre присвоено выбранное значение. Это динамически создаваемых разметки, выберите новый параметр нельзя просмотреть, просматривая разметку в браузере. Вы увидите новый HTML с помощью средств разработчика IE 9 F12. Для просмотра выберите новый параметр в Internet Explorer 9, нажмите клавишу F12 для запуска средств разработчика F12. Перейдите на страницу создания и добавить новый жанра, чтобы новый жанр выбран в списке выбора жанра. В средствах разработчика F12:

1. Перейдите на вкладку HTML.
2. Нажмите значок «Обновить».  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. В поле поиска введите GenreID.
4. Используя значок "Далее"   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Перейдите к выберите следующий тег:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Разверните последнее значение параметра.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

В него следующий код *Scripts\chooseGenre.js* файла показано, как **Добавление новой Genre** кнопку подключается к события щелчка кнопкой мыши и как **Добавление новой Genre** используется диалоговое окно создать.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

В первой строке создается щелкните функции, подключенный к **Добавление новой Genre** кнопки. Приведенная ниже разметка из Views\StoreManager\\_ChooseGenre.cshtml показан файл как **Добавление новой Genre** кнопка для:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Метод load создает и открывает диалоговое окно Добавить жанр и вызывает jQuery `parse` метод, поэтому проверка клиента выполняется на данные, введенные в диалоговом окне.

В этом разделе вы узнали, как для создания диалогового окна, который может использоваться для добавления новой категории данных в списке выбора. Необходимо выполнить той же процедуры для создания пользовательского интерфейса для добавления нового исполнителя в список выбора исполнителя. Этот учебник предоставил сведения о работе с вспомогательный метод ASP.NET MVC HTML **DropDownList**. Дополнительные сведения о работе с **DropDownList**, разделе «Добавление ссылки» ниже. Сообщите нам, если этого учебника было полезным.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Дополнительные ссылки

- [ASP.NET MVC — каскадные раскрывающихся списков учебника](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) по [Раду Энука](https://weblogs.asp.net/raduenuca/default.aspx)
- [Выбранный](http://harvesthq.github.com/chosen/) подключаемого модуля JavaScript, который поддерживает множественного выбора и фильтрации.

### <a name="contributors"></a>Авторы

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Брэд Вилсон](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Рецензенты

- Jean-Sébastien Goupil
- [Брэд Вилсон](http://bradwilson.typepad.com/)
- Эти протоколы Майк
- Tom Dykstra

> [!div class="step-by-step"]
> [Назад](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
