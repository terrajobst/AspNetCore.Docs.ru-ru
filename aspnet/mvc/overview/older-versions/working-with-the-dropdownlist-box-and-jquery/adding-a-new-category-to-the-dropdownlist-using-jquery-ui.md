---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Добавление новой категории в DropDownList с помощью пользовательский Интерфейс jQuery | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 5d23b1d41834d669bb8006ae0ac963fab60918dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402997"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Добавление новой категории в DropDownList с помощью пользовательского интерфейса jQuery
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

HTML-код `Select` тег идеально подходит для представления основных категории данных, но зачастую необходимо добавить новую категорию. Предположим, что мы хотим добавить жанр «Opera» в категории в нашей базе данных? В этом разделе мы будем использовать пользовательский Интерфейс jQuery, чтобы добавить диалоговое окно, можно использовать для добавления новой категории. На следующем рисунке показано, как пользовательский Интерфейс будет представлять в браузере.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Когда пользователь выбирает **добавить новый жанр** ссылку, всплывающее диалоговое окно запрашивает у пользователя имя жанр (и, при необходимости, описание). Ниже показано изображение **добавить жанр** всплывающее диалоговое окно.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Если введено новое имя жанр и **Сохранить** помещается кнопки, происходит следующее:

1. Вызов AJAX публикует данные для метода Create объекта контроллера жанра, который сохраняет новый жанр в базе данных и возвращает новый жанр сведения (имя жанр и идентификатора) как JSON.
2. JavaScript добавляют новые данные жанр к списку выбора.
3. JavaScript делает новый жанр, выбранный элемент.

   На рисунке ниже **Opera** был добавлен в базу данных и выбранного в **жанр** раскрывающегося списка. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Откройте *Views\StoreManager\Create.cshtml* файл и замените разметку жанр следующим следующий код:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Частичное представление будет содержать всю необходимую логику для подключения JavaScript и jQuery, используемый для реализации новой функции жанр add. Когда мы завершили код, он будет легко сделать то же самое с исполнителя пользовательского интерфейса.

В обозревателе решений щелкните правой кнопкой мыши *Views\StoreManager* папку и выберите **добавить**, затем **представление**. В **Имя_представления** ввода, введите `_ChooseGenre` выберите **добавить**. Замените существующую разметку в *Views\StoreManager\\_ChooseGenre.cshtml* файл со следующими:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

В первой строке объявляется, что мы передаем `Album` как модель, точно так же моделировать инструкции, обнаруженной в представление создания. Следующие несколько строк, **метка** разметке вспомогательной функции. Следующая строка представляет **DropDownList** вызвать вспомогательный, точно так же, как показано исходное представление создания. Следующая строка добавляет ссылку с именем `Add New Genre`, и стили его как кнопка. Строку, содержащую `ValidationMessageFor` копируется непосредственно из представления создания. Следующие строки:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Создает скрытый элемент div, с Идентификатором `genreDialog`. Мы будем использовать jQuery для подключения наших **добавить жанр** диалоговое окно с Идентификатором `genreDialog` в этот элемент div. Последние два теги сценариев содержат ссылки на файлы JavaScript, которые будут использоваться для реализации функции добавить новый жанра. */Scripts/chooseGenre.js* файл предоставляется для вас в проекте, мы рассмотрим его позже в этом руководстве.

Запустите приложение и щелкнуть **добавить новый жанр** кнопки. В **добавить жанр** диалогового окна введите **Opera** в **имя** поле ввода.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Нажмите кнопку **Сохранить**. Вызов AJAX создается категория Opera и затем заполняет раскрывающийся список с Opera и задает Opera как выбранный жанр.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Введите исполнителя, название и цену, а затем выберите **создать** кнопки. Если ввести цену, меньше $8.99, нового альбома будет отображаться в верхней части представления индекса. Убедитесь, что новая запись альбома был сохранен в базе данных.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Попробуйте создать новый жанр только одной буквы. В следующем коде в *Models\Genre.cs* файл задает минимальную и максимальную длину имени жанра.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Проверка на стороне клиента сообщает, что необходимо введите строку длиной от 2 до 20 символов.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Изучение как новый жанр добавляется в базу данных и поле со списком.

Откройте *Scripts\chooseGenre.js* файл и проанализировать код.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Вторая строка использует идентификатор `genreDialog` для создания диалогового окна в теге div в *Views\StoreManager\\_ChooseGenre.cshtml* файла. Большинство именованных параметров говорят сами за себя. `autoOpen` Параметр имеет значение false, выбрав **создать жанр** кнопки откроется диалоговое окно явно (описан в последнем). В диалоговом окне есть две кнопки **Сохранить** и **отменить**. **Отменить** кнопки закрывает диалоговое окно. В следующем коде показан **Сохранить** кнопку функции.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Нажимаете `createGenreForm` идентификатор. `createGenreForm` Идентификатор был задан в следующем коде в *Views\Genre\\_CreateGenre.cshtml* файла.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) вспомогательный перегруженный метод, используемый в *Views\Genre\\_CreateGenre.cshtml* файл создает HTML-код с атрибутом действие, содержащее URL-адрес для отправки формы. Это можно увидеть, отображается страница альбома "Создать" в браузере и выбрав show источника в браузере. В следующей разметке показан созданный код HTML, содержащий тег формы.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` строка делает вызов AJAX в атрибут действия (`/StoreManager/Create`) и передает данные из **создать жанр** диалоговое окно. Данные состоит из имени для нового жанр и необязательное описание. При успешном выполнении вызов AJAX новое имя жанр и значение добавляются к разметке выберите и новый жанр будет присвоено выбранное значение. Так как это динамически созданной разметки, выберите новый параметр не отображается, просматривая источник в обозревателе. Вы увидите новый HTML-код с помощью средств разработчика IE 9 F12. Чтобы просмотреть новый выберите параметр, в Internet Explorer 9, нажмите клавишу F12 для запуска в средствах разработчика F12. Перейдите на страницу создания и добавьте новый жанр фильма, чтобы новый жанр выбран в списке выбора жанра. В средствах разработчика F12:

1. Перейдите на вкладку HTML.
2. Нажмите значок обновления.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. В поле поиска введите GenreID.
4. С помощью значок "Далее"   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Перейдите к следующим тегом выберите:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Разверните последнее значение параметра.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

В следующем коде в *Scripts\chooseGenre.js* демонстрирует, как **добавить новый жанр** кнопку подключается к событие click и как **добавить новый жанр** используется диалоговое окно создан.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

В первой строке создается нажмите кнопку функции, подключенный к **добавить новый жанр** кнопки. Следующая разметка из Views\StoreManager\\_ChooseGenre.cshtml файла показан как **добавить новый жанр** создается кнопка:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Метод load создает и открывает диалоговое окно Добавить жанр и вызывает jQuery `parse` метод проверка клиента выполняется на данные, введенные в диалоговом окне.

В этом разделе вы узнали, как для создания диалогового окна, который может использоваться для добавления новых данных категории в списке выбора. Необходимо выполнить ту же процедуру для создания пользовательского интерфейса для добавления новых исполнителя в список выбора исполнителя. Приведенная в этом учебнике сведения о работе со вспомогательной функцией ASP.NET MVC HTML **DropDownList**. Дополнительные сведения о работе с **DropDownList**, см. раздел Добавление ссылки ниже. Сообщите нам, если этот учебник была для вас полезной.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Дополнительные ссылки

- [ASP.NET MVC — каскадные раскрывающиеся списки руководстве](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) по [Раду Энука](https://weblogs.asp.net/raduenuca/default.aspx)
- [Выбранная](http://harvesthq.github.com/chosen/) подключаемого модуля JavaScript, который поддерживает множественный выбор и фильтрации.

### <a name="contributors"></a>Авторы

- [Раду Энука](https://weblogs.asp.net/raduenuca/default.aspx)
- Жан Sébastien Goupil
- [Брэд Вилсон](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Рецензенты

- Жан Sébastien Goupil
- [Брэд Вилсон](http://bradwilson.typepad.com/)
- Эти протоколы Майк
- Том Дайкстра

> [!div class="step-by-step"]
> [Назад](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
