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
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Проверка того, как ASP.NET MVC формирует шаблоны вспомогательного метода DropDownList
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папку, а затем выберите **Добавление контроллера**. Назовите контроллер **StoreManagerController**. Для установки параметров **Добавление контроллера** диалоговое окно, как показано на рисунке ниже.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Изменить *StoreManager\Index.cshtml* Просмотр и удаление `AlbumArtUrl`. Удаление `AlbumArtUrl` будет сделать презентации более удобочитаемым. Ниже приведен готовый код.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Откройте *Controllers\StoreManagerController.cs* файл и найдите `Index` метод. Добавить `OrderBy` предложение, поэтому дисках будут отсортированы по цене. Ниже приведен полный код.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Сортировка по цене облегчит его проверить изменения в базу данных. При тестировании редактирования и создания методов, можно использовать низкая цена, сохраненные данные будут отображаться первыми.

Откройте *StoreManager\Edit.cshtml* файла. Добавьте следующую строку сразу после тега условных обозначений.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

В следующем коде показано контекст этого изменения:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Требуется вносить изменения в запись альбома.

Нажмите CTRL+F5, чтобы запустить приложение. Выберите, чтобы **администратора** ссылку, а затем выберите **Create New** связи для создания нового альбома. Убедитесь, что сохраненные сведения о диске. Изменить альбома и убедитесь, что внесенные изменения сохраняются.

### <a name="the-album-schema"></a>Схема альбома

`StoreManager` Контроллера, созданные с помощью механизма формирования шаблонов MVC разрешает доступ CRUD (Create, Read, Update, Delete) и альбомы, в базе данных магазина музыки. Ниже приводится схема для сведения об альбоме.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Таблице не хранятся жанр альбома и описание, он сохраняет внешний ключ к `Genres` таблицы. `Genres` Таблица содержит описание и название жанра. Аналогичным образом `Albums` таблица не содержит имя альбома исполнители, но внешний ключ к `Artists` таблицы. `Artists` Таблица содержит имя исполнителя. Если вы изучите данные в `Albums` таблицы, можно увидеть, каждая строка содержит внешний ключ к `Genres` таблицы и внешний ключ к `Artists` таблицы. На следующем рисунке показано некоторые данные таблицы из `Albums` таблицы.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Тег HTML Select

HTML-код `<select>` элемент (созданные HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) вспомогательный) используется для отображения полного списка значений (например, список жанров). Для формы редактирования когда известно текущее значение в списке выбора можно отобразить текущее значение. Мы видели ранее данном когда мы присвоено выбранное значение **комедия**. Список выбора идеально подходит для отображения категорий или данные внешнего ключа. `<select>` Элемент для внешнего ключа жанр отображает список имен возможных жанра, но обновляется при сохранении формы свойство жанр жанр значения внешнего ключа, не отображаемых жанр имя. В приведенном ниже рисунке является выбранного жанра **Disco** и исполнителя **лето Дарья**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Изучение ASP.NET MVC сформированные кода

Откройте *Controllers\StoreManagerController.cs* файл и найдите `HTTP GET Create` метод.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Метод складывает два [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) объектов `ViewBag`, в них сведения жанр и в них сведения исполнителя. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) перегрузку конструктора, выше принимает три аргумента:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *элементы*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) содержащий элементы в списке. В приведенном выше примере возвращаемый список жанров `db.Genres`.
2. *DataValueField используются*: имя свойства в **IEnumerable** список, содержащий значение ключа. В примере выше `GenreId` и `ArtistId`.
3. *dataTextField*: имя свойства в **IEnumerable** список, содержащий сведения для отображения. В таблице жанр и исполнители `name` используется поле.

Откройте *Views\StoreManager\Create.cshtml* файл и просмотрите `Html.DropDownList` разметке вспомогательной функции для жанра, полю.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Первая строка показывает, что требуется создать представление `Album` модели. В `Create` в приведенном выше примере, модель не был передан, поэтому представление получает **null** `Album` модели. На этом этапе мы при создании нового альбома, поэтому мы не установлены `Album` данных для него.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) перегрузка, показанный выше принимает имя поля для привязки к модели. Это имя также используется для поиска **ViewBag** объект, содержащий [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) объекта. Используя эту перегрузку, не требуется имя **ViewBag SelectList** объект `GenreId`. Второй параметр (`String.Empty`) — это текст, который отображается, если элемент не выбран. Это именно то, что необходимо при создании нового альбома. Если вы удаляли второго параметра и использовал следующий код:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Список выбора может по умолчанию первый элемент, то есть рок в нашем примере.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Изучение `HTTP POST Create` метод.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Эта перегрузка `Create` альбома `album` объект, созданный системой привязки модели ASP.NET MVC из формы значения, переданные. При отправке нового альбома, если состояние модели является допустимым и отсутствуют ошибки базы данных, нового альбома добавляется базы данных. Ниже показано создание нового альбома.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Можно использовать [инструмента fiddler](http://www.fiddler2.com/fiddler2/) изучаемый отправленные значения формы, привязку модели ASP.NET MVC использует для создания объекта альбома.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Рефакторинг, создание ViewBag SelectList

Оба `Edit` методы и `HTTP POST Create` метод имеют идентичный код для настройки **SelectList** в **ViewBag**. Из-за специфики ["не повторяйся"](http://en.wikipedia.org/wiki/Don't_repeat_yourself), мы будет рефакторинг данного кода. Мы сделаем это использование рефакторинг кода более поздней версии.

Создайте новый метод для добавления жанр и исполнителях **SelectList** для **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Замените две строки, параметр `ViewBag` в каждом из `Create` и `Edit` методы с помощью вызова `SetGenreArtistViewBag` метод. Ниже приведен готовый код.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Создание нового альбома и редактирование альбом, чтобы убедиться, что изменения работают.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Явно передающей SelectList в элемент управления DropDownList

Создание и изменение представления, созданные с помощью формирования шаблонов ASP.NET MVC следующие **DropDownList** перегрузки:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Разметка для представления создания, показано ниже.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Так как `ViewBag` свойство для `SelectList` называется `GenreId`, **DropDownList** будет использовать вспомогательные `GenreId` **SelectList** в **ViewBag** . В следующем **DropDownList** перегрузки, `SelectList` передается явно в.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Откройте *Views\StoreManager\Edit.cshtml* файлов и изменить **DropDownList** вызов явным образом передать **SelectList**, с помощью перегрузки выше. Это необходимо сделайте для категории по жанру. Ниже приведен полный код:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Запустите приложение и нажмите кнопку **администратора** ссылку, а затем перейдите к альбома Jazz и выберите **изменить** ссылку.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Вместо отображения Jazz как текущего выбранного жанра, рок отображается. Если строковый аргумент (свойство для привязки) и **SelectList** объекта имеют одинаковое имя, выбранное значение не используется. При отсутствии выбранных значение не указано, по умолчанию браузеры на первый элемент в **SelectList**(который является **рок** в приведенном выше примере). Это известное ограничение **DropDownList** вспомогательный.

Откройте *Controllers\StoreManagerController.cs* файлов и изменить **SelectList** имена для объектов `Genres` и `Artists`. Ниже приведен полный код:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Имена жанры и исполнители являются более понятные имена для категорий, так как они содержат больше, чем просто идентификатор каждой категории. Рефакторинг, которые мы делали ранее окупилась. Вместо изменения **ViewBag** в четыре метода изменения изолируются в `SetGenreArtistViewBag` метод.

Изменение **DropDownList** вызова в создание и изменение представлений для использования нового **SelectList** имена. Ниже показана новая разметка для представления изменения:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Создать представление требует пустую строку, чтобы предотвратить отображение первого элемента в SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Создание нового альбома и редактирование альбом, чтобы убедиться, что изменения работают. Тестировать код редактирования, выбрав альбом с жанра, отличный от рок.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>С помощью модели представления с помощью вспомогательного метода DropDownList

Создайте новый класс в папку ViewModels `AlbumSelectListViewModel`. Замените код в `AlbumSelectListViewModel` следующим кодом:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Конструктор принимает альбом, список исполнителей и жанров и создает объект, содержащий альбома и `SelectList` жанров и исполнителей.

Постройте проект таким образом `AlbumSelectListViewModel` доступна при следующем шаге мы создаем общее представление.

Добавить `EditVM` метод `StoreManagerController`. Ниже приведен готовый код.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Щелкните правой кнопкой мыши `AlbumSelectListViewModel`выберите **устранить**, затем **с помощью MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Кроме того, можно добавить следующий оператор using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Щелкните правой кнопкой мыши `EditVM` и выберите **Добавление представления**. Используйте параметры, приведенные ниже.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Выберите **добавить**, затем замените содержимое файла *Views\StoreManager\EditVM.cshtml* файл со следующими:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Разметки во многом схожа с исходным `Edit` разметки со следующими исключениями.

- Свойств в модели `Edit` представления имеют вид `model.property`(например, `model.Title` ). Свойств в модели `EditVm` представления имеют вид `model.Album.property`(например, `model.Album.Title`). Это потому, что `EditVM` представление передается контейнер `Album`, а не `Album` как в `Edit` представления.
- **DropDownList** второго параметра поступает из модели представления, не **ViewBag**.
- **BeginForm** вспомогательная функция в `EditVM` представления явно обратной передаче `Edit` метода действия. Публикуя обратно в `Edit` действия, мы не нужно писать `HTTP POST EditVM` действия могут использовать `HTTP POST` `Edit` действие.

Запустите приложение и изменить альбома. Изменить URL-адрес, используемый `EditVM`. Изменение поля и нажмите кнопку **Сохранить** кнопку, чтобы убедиться в работоспособности кода.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Какой подход следует использовать?

Все три подхода показано прерывая запись. Многие разработчики предпочитают explictily pass `SelectList` для `DropDownList` с помощью `ViewBag`. Такой подход имеет дополнительное преимущество дает гибкость использования более подходящего имени для коллекции. Единственным условием является нельзя присвоить имя `ViewBag SelectList` объекта совпадает с именем свойства модели.

Некоторые разработчики предпочитают подхода ViewModel. Другие рассмотрим более подробный разметку и создания кода HTML подхода ViewModel недостаток.

В этом разделе мы узнали три подхода к использованию **DropDownList** с категории данных. В следующем разделе будет показано, как добавить новую категорию.

> [!div class="step-by-step"]
> [Назад](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Вперед](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
