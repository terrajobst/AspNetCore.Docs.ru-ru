---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Изучение методов Edit и представления Edit | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасное и гораздо проще выполнить и демонстрационных версий...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: f8d66343ad74e45f167701f405c5e6e2fef0dd13
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824378"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Изучение методов Edit и представления Edit
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.


В этом разделе вы рассмотрите созданных методов и представлений действий для контроллера movie. Затем вы добавите пользовательской страницы поиска.

Запустите приложение и перейдите к `Movies` контроллер, добавляя */Movies* на URL-адрес в адресной строке браузера. Наведите указатель мыши на **изменить** ссылку, чтобы просмотреть URL-адрес, на которые она ссылается.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Изменить** ссылку созданное `Html.ActionLink` метод в *Views\Movies\Index.cshtml* представления:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Объект — это вспомогательный объект, который предоставляется с помощью свойства на [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) базового класса. `ActionLink` Метод вспомогательного приложения упрощает процесс динамического создания гиперссылки HTML, к методам действий на контроллерах. Первый аргумент `ActionLink` метод является текст ссылки для подготовки к просмотру (например, `<a>Edit Me</a>`). Вторым аргументом является имя вызываемого метода действия. Последний аргумент обозначает [анонимный объект](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , создающий данные маршрута (в нашем примере идентификатор 4).

Созданная ссылка, показанный на предыдущем рисунке — `http://localhost:xxxxx/Movies/Edit/4`. Маршрут по умолчанию (в *приложения\_Start\RouteConfig.cs*) принимает шаблон URL-адреса `{controller}/{action}/{id}`. Таким образом, преобразует ASP.NET `http://localhost:xxxxx/Movies/Edit/4` в запрос на `Edit` метод действия `Movies` контроллера с помощью параметра `ID` равным 4. Изучите следующий код из *приложения\_Start\RouteConfig.cs* файла.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Можно также передать параметрами метода действия, используя строку запроса. Например, URL-адрес `http://localhost:xxxxx/Movies/Edit?ID=4` также передает параметр `ID` из 4 `Edit` метод действия `Movies` контроллера.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Откройте `Movies` контроллера. Два `Edit` методы действий, показаны ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Обратите внимание на второй метод действия `Edit`, которому предшествует атрибут `HttpPost`. Этот атрибут указывает, что перегрузка `Edit` метод может вызываться только для запросов POST. Вы можете применить `HttpGet` атрибут к первому изменить метод, но это необязательно, так как он используется по умолчанию. (Я вернусь к методам действий, назначенных неявно `HttpGet` атрибут как `HttpGet` методов.)

`HttpGet` `Edit` Метод принимает параметр идентификатор фильма, выполняет поиск фильма с помощью Entity Framework `Find` метод и возвращает выбранный фильм в представление редактирования. Указывает параметр ID [значение по умолчанию](https://msdn.microsoft.com/library/dd264739.aspx) ноль, если `Edit` метод вызывается без параметров. Если не удается найти фильм, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) возвращается. Если в представлении редактирования создана система формирования шаблонов, она проверяет класс `Movie` и создает код для отображения элементов `<label>` и `<input>` для каждого свойства класса. В следующем примере показано представление редактирования, был создан:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Обратите внимание на то, каким образом шаблона представления содержится `@model MvcMovie.Models.Movie` инструкция в верхней части файла — это указывает, что в представлении требуется модель представления шаблона типа `Movie`.

В шаблонном коде используется несколько *вспомогательные методы* для оптимизации разметки HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Вспомогательный объект отображает имя поля (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;жанр&quot;, или &quot;цена &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Вспомогательный отображает HTML- `<input>` элемент. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Вспомогательный объект отображает любые сообщения проверки, связанные с этим свойством.

Запустите приложение и перейдите к */Movies* URL-адрес. Щелкните ссылку **Edit** (Изменить). Просмотрите исходный код страницы в окне браузера. Ниже приведен HTML для элемента form.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Элементы являются в HTML- `<form>` элемент которого `action` атрибут имеет значение для публикации */Movies/Edit* URL-адрес. Данные формы будут опубликованы на сервере при **изменить** кнопки.

## <a name="processing-the-post-request"></a>Обработка запроса POST

В следующем листинге демонстрируется версия `HttpPost` метода действия `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[Связывателя модели ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) принимает отправленные значения формы и создает `Movie` объект, который передается в качестве `movie` параметра. Метод `ModelState.IsValid` проверяет, можно ли использовать переданные в форме данные для изменения (редактирования или обновления) объекта `Movie`. Если данные являются допустимыми, данные фильма сохраняются `Movies` коллекцию `db(MovieDBContext` экземпляра). Новые данные фильма сохраняются в базу данных путем вызова `SaveChanges` метод `MovieDBContext`. После сохранения данных код перенаправляет пользователя для `Index` метод действия `MoviesController` класса, который отображает коллекцию фильмов, включая только что внесенных изменений.

Если переданные значения являются недопустимыми, они выводятся на экран в форме. `Html.ValidationMessageFor` Вспомогательные функции в *Edit.cshtml* представление шаблона позаботится об отображении соответствующие сообщения об ошибках.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> Поддержка проверки jQuery для английского, используйте запятую (&quot;,&quot;) для десятичной запятой, необходимо включить *globalize.js* и конкретными *cultures/globalize.cultures.js* файла (из [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`. В следующем коде показано изменения в файл Views\Movies\Edit.cshtml для работы с &quot;fr-FR&quot; языка и региональных параметров:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Десятичное поле требуется запятая, а не десятичной запятой. Как временное решение можно добавить элемент глобализации в файле web.config проектов. В следующем коде показано элементе globalization с языком и региональными параметрами, задайте для английского (США).

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Все `HttpGet` методы следуют одной схеме. Они получают объект фильма (или список объектов, в случае `Index`) и передачи в представление. `Create` Метод передает пустой объект фильма представления создания. Все методы, которые создают, редактируют, удаляют или иным образом изменяют данные, делают это в перегрузке метода `HttpPost`. Изменение данных в методе HTTP GET представляет угрозу безопасности, как описано в записи блога post [ASP.NET MVC совет #46 — не использовать ссылки, удалить, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Изменение данных в метод GET также не соответствует рекомендациям по HTTP и архитектуры [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) шаблона, которое указывает, что запросы GET не должны изменять состояние приложения. Другими словами, операция GET должна выполняться безопасным способом, то есть не иметь побочных эффектов и не изменять существующие данные.

## <a name="adding-a-search-method-and-search-view"></a>Добавление метода поиска и поиска представления

В этом разделе вы добавите `SearchIndex` метод действия, который позволяет выполнять поиск фильмов по жанру или имени. Это будет доступно при использовании */Movies/SearchIndex* URL-адрес. Запрос отобразит форму HTML, который содержит входные элементы, которые пользователь может ввести для поиска фильмов. Когда пользователь отправляет форму, метод действия получения опубликованных пользователем значения для поиска и использовать значения, чтобы найти в базе данных.

## <a name="displaying-the-searchindex-form"></a>Отображение формы SearchIndex

Начните с добавления `SearchIndex` метода действия к существующему `MoviesController` класса. Метод возвращает представление, содержащее HTML-форму. Ниже приведен код:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Первая часть `SearchIndex` метод создает следующие [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрос для выбора фильмов:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Запрос на этом этапе определяется, но еще не было запущено в хранилище данных.

Если `searchString` содержит строку, запрос фильмов изменяется для фильтрации по значению в строке поиска, используя следующий код:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Приведенный выше код `s => s.Title` представляет собой [лямбда-выражение](https://msdn.microsoft.com/library/bb397687.aspx). Лямбда-выражения используются в основе метод [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрашивает в качестве аргументов стандартных методов операторов запроса, такие как [где](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) метод, используемый в приведенном выше коде. Запросы LINQ не выполняются, когда они определяются или изменяются путем вызова метода, таких как `Where` или `OrderBy`. Вместо этого выполнение запроса откладывается, это означает, что вычисление выражения откладывается, пока его реализованного значения будет выполнена итерация или [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) вызывается метод. В `SearchIndex` пример, в представлении SearchIndex выполняется запрос. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/library/bb738633.aspx).

Теперь вы можете реализовать `SearchIndex` представление, которое будет отображать форму для пользователя. Щелкните правой кнопкой мыши внутри `SearchIndex` метод и нажмите кнопку **Добавление представления**. В **Добавление представления** диалоговом окне укажите, что вы собираетесь передать `Movie` объект в шаблоне представления, что и класс модели. В **шаблона каркаса** выберите **списка**, затем нажмите кнопку **добавить**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

При нажатии кнопки **добавить** кнопки *Views\Movies\SearchIndex.cshtml* создается шаблон представления. Так как вы выбрали **списка** в **шаблона каркаса** список, Visual Studio автоматически создается (сформированные) некоторые разметки по умолчанию в представлении. Формирование шаблонов создана HTML-форму. Отдавайте его `Movie` класс и создает код для отображения `<label>` элементы для каждого свойства класса. В следующем списке показана представления создания, который был создан:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Запустите приложение и перейдите к */Movies/SearchIndex*. Добавьте в URL-адрес строку запроса, например `?searchString=ghost`. Отображаются отфильтрованные фильмы.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Если вы изменили сигнатуру `SearchIndex` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набора в *Global.asax* файл.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Исходный `SearchIndex` метод выглядит следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Измененный `SearchIndex` метода будет выглядеть следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов. Итак, теперь мы добавим пользовательский Интерфейс для удобства их фильтрации фильмов. Если вы изменили сигнатуру `SearchIndex` метод для тестирования передачи параметра ID привязкой к маршруту, измените его, чтобы ваши `SearchIndex` метод принимает строковый параметр с именем `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Откройте *Views\Movies\SearchIndex.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте следующий код:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

В следующем примере показано часть *Views\Movies\SearchIndex.cshtml* файл добавлена фильтрации разметкой.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Вспомогательный создает открывающий `<form>` тега. `Html.BeginForm` Вспомогательный вызывает формы post к самому себе, когда пользователь отправляет форму, щелкнув **фильтра** кнопки.

Запустите приложение и попробуйте выполнить поиск фильма.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Существует не `HttpPost` перегрузки `SearchIndex` метод. Это не требуется, поскольку метод не изменяет состояние приложения, просто выполняет фильтрацию данных.

Можно добавить следующий метод `HttpPost SearchIndex`. В этом случае средство вызова действий будет соответствовать `HttpPost SearchIndex` метод и `HttpPost SearchIndex` метод будет выполняться, как показано на рисунке ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Тем не менее при добавлении этой версии `HttpPost` метода `SearchIndex` существует ограничение на общую реализацию. Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов. Обратите внимание на то, что URL-адрес запроса HTTP POST совпадает со значением URL-адрес для запроса GET (localhost: xxxxx/Movies/SearchIndex) — нет поиска информации в сам URL-адрес. Справа, данные строки поиска отправляется на сервер как значения поля формы. Это означает, что вы не можете передавать эту информацию поиска закладки или отправить друзьям в URL-адрес.

Рекомендуется использовать перегрузку `BeginForm` , указывающий, что запрос POST должен добавлять сведения о поиске URL-адрес и что оно должно быть направлено на HttpGet версию `SearchIndex` метод. Замените существующий без параметров `BeginForm` следующим кодом:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Теперь при отправки поиска URL-адрес содержит строку запроса поиска. Поиск также переносится в метод `HttpGet SearchIndex`, даже если у вас определен метод `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Добавление поиска по жанру

Если вы добавили `HttpPost` версии `SearchIndex` метод, удалить его сейчас.

Далее добавим функцию, которая позволит пользователям поиск фильмов по жанру. Замените метод `SearchIndex` следующим кодом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Эта версия `SearchIndex` метод принимает дополнительный параметр, а именно `movieGenre`. Создать первые несколько строк кода `List` объекта, содержащего жанров фильма из базы данных.

Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех отдельных жанров. (Без `Distinct` модификатор, будут добавлены повторяющиеся жанры — например, комедия будут добавлены в нашем примере дважды). Код сохраняет список жанров в `ViewBag` объекта.

Следующий код показывает, как проверить `movieGenre` параметра. Если значение не пустое, код ограничивает запрос для ограничения выбранный фильм, чтобы указанный жанр фильмов.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Добавив разметку в представление SearchIndex для поддержки поиска по жанру

Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\SearchIndex.cshtml* файла, непосредственно перед `TextBox` вспомогательный. Ниже приводится полная разметка.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Запустите приложение и перейдите к */Movies/SearchIndex*. Попробуйте выполнить поиск по жанру, название фильма и по обоим критериям.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

В этом разделе рассмотрено действия CRUD-методы и представления, сформированные платформой. Вы создали метод действия поиска и представления, которые позволяют пользователям выполнять поиск по названию фильма и жанр. В следующем разделе будут рассмотрены способы добавьте свойство, чтобы `Movie` модель и как добавить инициализатор, который автоматически создает тестовую базу данных.

> [!div class="step-by-step"]
> [Назад](accessing-your-models-data-from-a-controller.md)
> [Вперед](adding-a-new-field-to-the-movie-model-and-table.md)
