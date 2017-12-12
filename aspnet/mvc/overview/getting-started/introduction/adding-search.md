---
uid: mvc/overview/getting-started/introduction/adding-search
title: "Поиск | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: a7664d16a056424ee51db2208152cb5d35d8e5d9
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2017
---
<a name="search"></a>Поиск
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Добавление метода поиска и просмотра поиска

В этом разделе вы добавите возможностей поиска в `Index` метод действия, который позволяет искать фильмов по жанру или имени.

## <a name="updating-the-index-form"></a>Обновление индекса формы

Начните с обновления `Index` метода действия для существующего `MoviesController` класса. Ниже приведен код:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

Первая строка `Index` метод создает следующие [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) запрос, чтобы выбрать фильмы:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Запрос на этом этапе определяется, но еще не было запущено в базе данных.

Если `searchString` параметр содержит строку, запрос фильмы изменяется для фильтрации по значению строки поиска, используя следующий код:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Приведенный выше код `s => s.Title` представляет собой [лямбда-выражение](https://msdn.microsoft.com/en-us/library/bb397687.aspx). Лямбда-выражения, используемые в основе метода [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) запрашивает качестве аргументов стандартных методов операторов запроса, таких как [где](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) метод, используемый в приведенном выше коде. LINQ запросы не выполняются при их определении либо изменении путем вызова метода, например `Where` или `OrderBy`. Вместо этого выполнение запроса отложено, это означает, что вычисление выражения откладывается до его реализованных значение фактически итерации или [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) вызывается метод. В `Search` образца, запрос выполняется в *Index.cshtml* представления. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/en-us/library/bb738633.aspx).

> [!NOTE]
> [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) метод выполняется в базе данных, а не в коде c# выше. В базе данных [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) сопоставляется [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), которая не учитывает регистр.

Теперь вы можете обновить `Index` представление, которое будет отображать формы для пользователя.

Запустите приложение и перейдите к */фильмов/индекса*. Добавьте в URL-адрес строку запроса, например `?searchString=ghost`. Отображаются отфильтрованные фильмы.

![SearchQryStr](adding-search/_static/image1.png)

Если изменить подпись `Index` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набор в *приложения\_сценарий RouteConfig.cs* файл.

[!code-json[Main](adding-search/samples/sample4.json)]

Исходный `Index` метод выглядит следующим образом:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Измененный `Index` метода будет выглядеть следующим образом:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.

![](adding-search/_static/image2.png)

Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов. Поэтому теперь вы добавите пользовательского интерфейса для их фильтрации фильмов. Если изменить подпись `Index` метод для проверки как передавать параметр ID привязкой маршрута, изменить его, чтобы вашей `Index` метод принимает строковый параметр с именем `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Откройте *Views\Movies\Index.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте разметку формы, выделены ниже:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Вспомогательный создает открывающий `<form>` тег. `Html.BeginForm` Вспомогательный вызывает формы post на самого себя, когда пользователь отправляет форму, нажав кнопку **фильтра** кнопки.

Visual Studio 2013 имеет значительное улучшение работы с низким приоритетом при отображении и изменении Просмотр файлов. При запуске приложения с помощью файла представления open, Visual Studio 2013 вызывает метод действия контроллера правильный представление.

![](adding-search/_static/image3.png)

Представление Index открыт в Visual Studio (как показано на приведенном выше рисунке), коснитесь Ctr F5 или F5, чтобы запустить приложение и повторите поиск фильма.

![](adding-search/_static/image4.png)

Имеется не `HttpPost` перегрузки `Index` метод. Это не требуется, поскольку метод не изменяет состояние приложения, просто фильтрации данных.

Можно добавить следующий метод `HttpPost Index`. В этом случае средство вызова действий будет соответствовать `HttpPost Index` метода и `HttpPost Index` метод будет выполняться, как показано на рисунке ниже.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Тем не менее при добавлении этой версии `HttpPost` метода `Index` существует ограничение на общую реализацию. Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов. Обратите внимание, что URL-адрес для запроса HTTP POST совпадает с URL-адрес для запроса на получение (localhost:xxxxx/фильмов/индекса) — нет поиска сведений в самом URL-АДРЕСЕ. Справа теперь данные строки поиска отправляется на сервер как значения поля формы. Это означает, что вы не можете получить эту информацию поиска для закладки или отправить друзьям в URL-АДРЕСЕ.

Рекомендуется использовать перегрузку `BeginForm` , указывающий, что запрос POST следует добавить поиск сведений о URL-адрес и должно быть направлено в `HttpGet` версии `Index` метод. Заменить существующий без параметров `BeginForm` метод следующей разметкой:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Теперь при отправке поиска, URL-адрес содержит строку запроса поиска. Поиск также переносится в метод `HttpGet Index`, даже если у вас определен метод `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Добавление поиска по жанру

Если вы добавили `HttpPost` версии `Index` метод, сейчас ее удалить.

Далее можно добавить функцию, чтобы позволить пользователям поиск фильмов по жанру. Замените метод `Index` следующим кодом:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Эта версия `Index` метод принимает дополнительный параметр, а именно `movieGenre`. Первые несколько строк кода, создание `List` объекта, содержащего жанров фильмов из базы данных.

Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех уникальных жанров. (Без `Distinct` модификатор, следует добавить повторяющийся жанров — например, комедия добавляется дважды в нашем примере). Код сохраняет список жанров в `ViewBag.movieGenre` объекта. Хранение данных категории (такие фильма жанра) как [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) объекта в `ViewBag`, то доступ к данным категории в раскрывающемся списке — типичный подход для приложений MVC.

В следующем коде показано, как проверить `movieGenre` параметра. Если она не пустая, код далее ограничивает запрос фильмов с целью ограничения фильмы, выбранных для заданного жанра.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Как уже говорилось ранее, запрос выполняется не на базе данных, пока циклах список фильмов (в представлении, которое производится после `Index` возвращает метод действия).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Добавление разметки в представлении индекса для поддержки поиска по жанру

Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\Index.cshtml* файла, непосредственно перед `TextBox` вспомогательный. Ниже приводится полная разметка.

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

В следующем коде:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Параметр «movieGenre» предоставляет ключ для `DropDownList` вспомогательный метод для поиска `IEnumerable<SelectListItem>` в `ViewBag`. `ViewBag` Была заполнена в методе действия:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Параметр «All» предоставляет метку варианта. Если проверить выбор в браузере, вы увидите, что его атрибут «value» является пустым. Поскольку наш контроллера только фильтрует `if` строка не `null` или является пустым, отправка пустое значение для `movieGenre` показывает все жанров.

Можно также установить параметр выбран по умолчанию. Если требуется «Комедия» в качестве дополнения по умолчанию, необходимо изменить код в контроллере следующим образом:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Запустите приложение и перейдите к */фильмов/индекса*. Попробуйте выполните поиск по жанру, названия фильма и обоим критериям.

![](adding-search/_static/image8.png)

В этом разделе вы создали поиск метода действия и представления, которые позволяют пользователям выполнять поиск по название фильма и жанр. В следующем разделе будут рассмотрены способы Добавление свойства `Movie` модели и как добавить инициализатор, который автоматически создает тестовую базу данных.

>[!div class="step-by-step"]
[Назад](examining-the-edit-methods-and-edit-view.md)
[Вперед](adding-a-new-field.md)
