---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "Изучение как scaffolds вспомогательного метода DropDownList в ASP.NET MVC | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: b5210f9a29f82fbadd0e6dd2d81bd85e7f23ae7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Изучение как scaffolds вспомогательного метода DropDownList в ASP.NET MVC
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папки, а затем выберите **добавить контроллер**. Назовите контроллер **StoreManagerController**. Для установки параметров **добавить контроллер** диалогового окна, как показано на рисунке ниже.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Изменить *StoreManager\Index.cshtml* Просмотр и удаление `AlbumArtUrl`. Удаление `AlbumArtUrl` будет сделать презентации более удобочитаемым. Ниже приведен полный код.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Откройте *Controllers\StoreManagerController.cs* файла и найти `Index` метод. Добавить `OrderBy` предложение, дисках будут отсортированы по цене. Ниже приведен полный код.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Сортировка по цене поможет упростить его для проверки изменений в базу данных. При тестировании изменения и создать методы можно использовать низкая цена, сохраненные данные будут отображаться первыми.

Откройте *StoreManager\Edit.cshtml* файла. Добавьте следующую строку сразу после тега условных обозначений.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

В следующем коде показано контекст этого изменения:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Требуется внести изменения в записи альбома.

Нажмите CTRL+F5, чтобы запустить приложение. Выберите, чтобы **администратора** связь, а затем выберите **создать новый** ссылку, чтобы создать нового альбома. Убедитесь, что сведения о диске был сохранен. Изменить альбом и убедитесь, что внесенные изменения сохраняются.

### <a name="the-album-schema"></a>Альбом схемы

`StoreManager` Контроллера, созданного с помощью механизма формирования шаблонов MVC разрешает доступ CRUD (Создание, чтение, обновление, удаление) на дисках в хранилище Музыкальная база данных. Ниже приводится схема для сведений о диске.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Таблице не хранятся жанр альбом и описание, он сохраняет внешний ключ к `Genres` таблицы. `Genres` Таблица содержит жанр имя и описание. Аналогичным образом `Albums` таблица не будет содержать имя альбома исполнители, но внешний ключ к `Artists` таблицы. `Artists` Таблица содержит имя исполнителя. Если изучить данные в `Albums` таблицы, можно увидеть, каждая строка содержит внешний ключ к `Genres` таблицы и внешний ключ к `Artists` таблицы. На рисунке ниже отображаются некоторые данные таблицы из `Albums` таблицы.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Выберите HTML-тег

HTML `<select>` элемент (созданные HTML [DropDownList](https://msdn.microsoft.com/en-us/library/dd492948.aspx) вспомогательный) используется для отображения полного списка значений (например, список жанров). Для изменения формы когда текущее значение известно, список выбора можно отобразить текущее значение. Мы видели этом ранее при рекомендуется задать выбранное значение значение **комедии**. Список выбора идеально подходит для отображения категорий или данные внешнего ключа. `<select>` Элемент внешнего ключа жанра отображает список имен возможных жанра, но обновляется при сохранении формы свойство жанр с Genre значение внешнего ключа, не отображаемых жанр имя. На приведенном ниже рисунке выбран жанр — **Disco** и исполнителя **лето Дарья**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Проверки ASP.NET MVC формирования шаблонов кода

Откройте *Controllers\StoreManagerController.cs* файла и найти `HTTP GET Create` метод.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Метод добавляет два [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist.aspx) объектов `ViewBag`, один для хранения сведений жанр и один для хранения данных исполнителя. [SelectList](https://msdn.microsoft.com/en-us/library/dd505286.aspx) перегрузку конструктора, приведенном выше принимает три аргумента:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *элементы*: [IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable.aspx) содержащие элементы в списке. В приведенном выше примере список жанров, возвращаемый методом `db.Genres`.
2. *DataValueField используются*: имя свойства в **IEnumerable** список, содержащий значение ключа. В приведенном выше примере `GenreId` и `ArtistId`.
3. *dataTextField*: имя свойства в **IEnumerable** список, который содержит сведения для отображения. В таблице жанр и исполнители `name` используется поле.

Откройте *Views\StoreManager\Create.cshtml* файл и проверьте `Html.DropDownList` вспомогательный разметку для жанра, полю.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Первая строка показывает, что требуется создать представление `Album` модели. В `Create` в приведенном выше примере, модель не были переданы, поэтому представление возвращает **null** `Album` модели. На этом этапе создается новый альбом, поэтому мы не установлены `Album` данных для него.

[Html.DropDownList](https://msdn.microsoft.com/en-us/library/dd492948.aspx) перегрузка, показанный выше принимает имя поля для привязки модели. Также использует это имя для поиска **ViewBag** объект, содержащий [SelectList](https://msdn.microsoft.com/en-us/library/dd505286.aspx) объекта. С помощью этой перегрузки, не требуется имя **ViewBag SelectList** объекта `GenreId`. Второй параметр (`String.Empty`) — это текст, отображаемый, когда элемент не выбран. Это именно то, что необходимо при создании нового альбома. Если удалить второй параметр и использовать следующий код:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Список выбора может по умолчанию первый элемент или рок выборка.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Изучение `HTTP POST Create` метод.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Эта перегрузка `Create` принимает `album` объект, созданный системой привязки модели ASP.NET MVC от формы значений, переданных. При отправке нового альбома, если состояние модели является допустимым, и нет ли ошибок базы данных нового альбома добавляется в базу данных. Ниже показано создание нового альбома.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Можно использовать [инструмента fiddler](http://www.fiddler2.com/fiddler2/) для проверки значений, отправленной формы этой привязки модели ASP.NET MVC использует для создания объекта альбома.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Создание ViewBag SelectList рефакторинга

Оба `Edit` методы и `HTTP POST Create` метод имеют идентичный код, чтобы настроить **SelectList** в **ViewBag**. В духе [ТОНЕРА](http://en.wikipedia.org/wiki/Don't_repeat_yourself), рефакторинга этот код. Мы сделаем это использование позже рефакторинга кода.

Создайте новый метод для добавления жанр и исполнитель **SelectList** для **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Замените две строки, параметр `ViewBag` в каждом из `Create` и `Edit` методов с помощью вызова `SetGenreArtistViewBag` метод. Ниже приведен полный код.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Создание нового альбома и изменение альбом, чтобы убедиться, что изменения работают.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Явно передачи SelectList DropDownList

Создание и изменение представления, созданные с помощью ASP.NET MVC функции формирования шаблонов следующие **DropDownList** перегрузки:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Разметка для представления создания, показано ниже.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Поскольку `ViewBag` свойство `SelectList` называется `GenreId`, **DropDownList** будет использовать вспомогательный `GenreId` **SelectList** в **ViewBag** . В следующем **DropDownList** перегрузки, `SelectList` передается в явном виде.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Откройте *Views\StoreManager\Edit.cshtml* и измените **DropDownList** вызов явным образом передать в **SelectList**, с помощью перегрузки выше. Это необходимо сделайте для категории по жанру. Ниже приведен полный код:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Запустите приложение и нажмите кнопку **администратора** связь, а затем перейдите в альбоме Jazz и выберите **изменить** ссылку.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Вместо демонстрации Jazz как выбранного жанра, отображается рок. Если строковый аргумент (свойство для привязки) и **SelectList** объект имеет то же имя, выбранное значение не используется. При отсутствии выбранных значение не указано, по умолчанию браузеры на первый элемент в **SelectList**(который является **рок** в приведенном выше примере). Это известное ограничение **DropDownList** вспомогательный.

Откройте *Controllers\StoreManagerController.cs* и измените **SelectList** имена для объектов `Genres` и `Artists`. Ниже приведен полный код:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Имена жанров и исполнителей, лучше имена для категорий, они содержат больше, чем просто идентификатор каждой категории. Рефакторинг, которые мы делали ранее платные. Вместо изменения **ViewBag** в четыре метода изменения изолируются в `SetGenreArtistViewBag` метод.

Изменение **DropDownList** вызова создания и изменения представлений, чтобы использовать новую **SelectList** имена. Ниже показан новый разметка для представления изменения:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Создать представление требует пустую строку, чтобы предотвратить отображение первого элемента в SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Создание нового альбома и изменение альбом, чтобы убедиться, что изменения работают. Тестирование кода редактирования, выбрав альбом с жанра, отличные от рок.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Использование модели представления с вспомогательного метода DropDownList

Создайте новый класс в папке ViewModels с именем `AlbumSelectListViewModel`. Замените код в `AlbumSelectListViewModel` класса следующими строками:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Конструктор принимает альбома, список исполнителей и жанров и создает объект, содержащий альбом и `SelectList` жанров и исполнителей.

Выполните построение проекта поэтому `AlbumSelectListViewModel` доступен при создании представления на следующем шаге.

Добавить `EditVM` метод `StoreManagerController`. Ниже приведен полный код.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Щелкните правой кнопкой мыши `AlbumSelectListViewModel`выберите **Разрешить**, затем **MvcMusicStore.ViewModels; с помощью**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Кроме того, можно добавить следующие с помощью инструкции:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Щелкните правой кнопкой мыши `EditVM` и выберите **добавить представление**. Используйте параметры, приведенные ниже.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Выберите **добавить**, затем замените содержимое *Views\StoreManager\EditVM.cshtml* файл со следующим:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Разметки очень похожа на исходный `Edit` разметки со следующими исключениями.

- Свойств в модели `Edit` представления имеют форму `model.property`(например, `model.Title` ). Свойств в модели `EditVm` представления имеют форму `model.Album.property`(например, `model.Album.Title`). Причина этого заключается в `EditVM` представления передается контейнер `Album`, а не `Album` в качестве `Edit` представления.
- **DropDownList** второй параметр не поступают из модели представления **ViewBag**.
- **BeginForm** вспомогательный в `EditVM` представление явно обратной передаче `Edit` метода действия. Публикуя обратно в `Edit` действия, нам не нужно писать `HTTP POST EditVM` действия могут использовать `HTTP POST` `Edit` действия.

Запустите приложение и изменять альбома. Изменить URL-адрес для использования `EditVM`. Изменение поля и нажмите кнопку **Сохранить** кнопку, чтобы убедиться, что код работает.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Какой подход следует использовать?

Все три показано из них acceptible. Многие разработчики предпочитают проход explictily `SelectList` для `DropDownList` с помощью `ViewBag`. Данный подход имеет дополнительное преимущество это обеспечивает гибкость применения более подходящее имя для коллекции. Необходимо учитывать — не могут иметь имена `ViewBag SelectList` объекта совпадает с именем свойства модели.

Некоторые разработчики предпочитают ViewModel подход. Рассмотрите возможность более подробных сведений другим пользователям разметки и созданный HTML ViewModel приближаться к отметке недостаток.

В этом разделе мы узнали три подхода к использованию **DropDownList** категории данных. В следующем разделе мы покажем, как добавить новую категорию.

>[!div class="step-by-step"]
[Назад](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Вперед](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
