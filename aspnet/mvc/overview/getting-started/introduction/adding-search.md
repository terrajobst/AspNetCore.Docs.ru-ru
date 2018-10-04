---
uid: mvc/overview/getting-started/introduction/adding-search
title: Поиск | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 2cf2274a5592e1f073e62c9b8a789fbb61e23a51
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576382"
---
<a name="search"></a>Поиск
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Добавление метода поиска и поиска представления

В этом разделе вы добавите возможность поиска `Index` метод действия, который позволяет выполнять поиск фильмов по жанру или имени.

## <a name="updating-the-index-form"></a>Обновления формы индекса

Начните с обновления `Index` метода действия к существующему `MoviesController` класса. Ниже приведен код:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

Первая часть `Index` метод создает следующие [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрос для выбора фильмов:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Запрос на этом этапе определяется, но еще не было выполнено в базе данных.

Если `searchString` содержит строку, запрос фильмов изменяется для фильтрации по значению в строке поиска, используя следующий код:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Приведенный выше код `s => s.Title` представляет собой [лямбда-выражение](https://msdn.microsoft.com/library/bb397687.aspx). Лямбда-выражения используются в основе метод [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрашивает в качестве аргументов стандартных методов операторов запроса, такие как [где](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) метод, используемый в приведенном выше коде. Запросы LINQ не выполняются, когда они определяются или изменяются путем вызова метода, таких как `Where` или `OrderBy`. Вместо этого выполнение запроса откладывается, это означает, что вычисление выражения откладывается, пока его реализованного значения будет выполнена итерация или [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) вызывается метод. В `Search` примере запрос выполняется в *Index.cshtml* представления. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> [Contains](https://msdn.microsoft.com/library/bb155125.aspx) метод выполняется на базе данных, а не код c# выше. В базе данных [Contains](https://msdn.microsoft.com/library/bb155125.aspx) сопоставляется [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), который не учитывает регистр.

Теперь вы можете обновить `Index` представление, которое будет отображать форму для пользователя.

Запустите приложение и перейдите к */Movies/Index*. Добавьте в URL-адрес строку запроса, например `?searchString=ghost`. Отображаются отфильтрованные фильмы.

![SearchQryStr](adding-search/_static/image1.png)

Если вы изменили сигнатуру `Index` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набора в *приложения\_функции RouteConfig.cs* файл.

[!code-json[Main](adding-search/samples/sample4.json)]

Исходный `Index` метод выглядит следующим образом:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Измененный `Index` метода будет выглядеть следующим образом:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.

![](adding-search/_static/image2.png)

Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов. Итак, теперь мы добавим пользовательский Интерфейс для удобства их фильтрации фильмов. Если вы изменили сигнатуру `Index` метод для тестирования передачи параметра ID привязкой к маршруту, измените его, чтобы ваши `Index` метод принимает строковый параметр с именем `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Откройте *Views\Movies\Index.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте разметку формы, описанных ниже:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Вспомогательный создает открывающий `<form>` тега. `Html.BeginForm` Вспомогательный вызывает формы post к самому себе, когда пользователь отправляет форму, щелкнув **фильтра** кнопки.

Visual Studio 2013 имеет значительное усовершенствование при отображения и редактирования файлов представлений. При запуске приложения с помощью файла представления откройте Visual Studio 2013 вызывает метод действия правильный контроллера для отображения представления.

![](adding-search/_static/image3.png)

Представление Index открыт в Visual Studio (как показано на приведенном выше рисунке), коснитесь Ctr F5 или F5, чтобы запустить приложение и повторите поиск фильма.

![](adding-search/_static/image4.png)

Существует не `HttpPost` перегрузки `Index` метод. Это не требуется, поскольку метод не изменяет состояние приложения, просто выполняет фильтрацию данных.

Можно добавить следующий метод `HttpPost Index`. В этом случае средство вызова действий будет соответствовать `HttpPost Index` метод и `HttpPost Index` метод будет выполняться, как показано на рисунке ниже.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Тем не менее при добавлении этой версии `HttpPost` метода `Index` существует ограничение на общую реализацию. Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов. Обратите внимание на то, что URL-адрес запроса HTTP POST совпадает со значением URL-адрес для запроса GET (localhost: xxxxx/Movies/Index) — нет поиска информации в сам URL-адрес. Справа, данные строки поиска отправляется на сервер как значения поля формы. Это означает, что вы не можете передавать эту информацию поиска закладки или отправить друзьям в URL-адрес.

Рекомендуется использовать перегрузку `BeginForm` , указывающий, что запрос POST должен добавлять сведения о поиске URL-адрес и что оно должно быть направлено `HttpGet` версии `Index` метод. Замените существующий без параметров `BeginForm` метод, используя следующую разметку:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Теперь при отправки поиска URL-адрес содержит строку запроса поиска. Поиск также переносится в метод `HttpGet Index`, даже если у вас определен метод `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Добавление поиска по жанру

Если вы добавили `HttpPost` версии `Index` метод, удалить его сейчас.

Далее добавим функцию, которая позволит пользователям поиск фильмов по жанру. Замените метод `Index` следующим кодом:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Эта версия `Index` метод принимает дополнительный параметр, а именно `movieGenre`. Создать первые несколько строк кода `List` объекта, содержащего жанров фильма из базы данных.

Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех отдельных жанров. (Без `Distinct` модификатор, будут добавлены повторяющиеся жанры — например, комедия будут добавлены в нашем примере дважды). Код сохраняет список жанров в `ViewBag.MovieGenre` объекта. Хранение данных категории (такие фильма жанр фильма) как [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) объекта в `ViewBag`, то доступ к данным категорию в раскрывающемся списке является типичным подходом для приложений MVC.

Следующий код показывает, как проверить `movieGenre` параметра. Если значение не пустое, код ограничивает запрос для ограничения выбранный фильм, чтобы указанный жанр фильмов.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Как уже говорилось ранее, запрос выполняется не на базе данных, пока выполняется итерация списка фильмов (что происходит в представлении после `Index` метод действия возвращает).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Добавив разметку в представление Index для поддержки поиска по жанру

Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\Index.cshtml* файла, непосредственно перед `TextBox` вспомогательный. Ниже приводится полная разметка.

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

В следующем коде:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Параметр «MovieGenre» предоставляет ключ для `DropDownList` вспомогательный метод для поиска `IEnumerable<SelectListItem>` в `ViewBag`. `ViewBag` Была заполнена в методе действия:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Параметр «All» предоставляет метку варианта. Если проверить этот выбор в браузере, вы увидите, что его атрибут «value» пуста. Поскольку нашего контроллера только фильтрует `if` строка не `null` или empty, отправка пустое значение для `movieGenre` показывает все жанров.

Можно также задать параметр выбран по умолчанию. Если требуется «Комедия» как параметры по умолчанию, необходимо изменить код в контроллере следующим образом:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Запустите приложение и перейдите к */Movies/Index*. Попробуйте выполнить поиск по жанру, название фильма и по обоим критериям.

![](adding-search/_static/image8.png)

В этом разделе вы создали метод действия поиска и представления, которые позволяют пользователям выполнять поиск по названию фильма и жанр. В следующем разделе будут рассмотрены способы добавьте свойство, чтобы `Movie` модель и как добавить инициализатор, который автоматически создает тестовую базу данных.

> [!div class="step-by-step"]
> [Назад](examining-the-edit-methods-and-edit-view.md)
> [Вперед](adding-a-new-field.md)
