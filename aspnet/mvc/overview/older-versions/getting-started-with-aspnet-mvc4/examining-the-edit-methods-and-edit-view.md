---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: "Изучение методы изменения и представления изменения | Документы Microsoft"
author: Rick-Anderson
description: "Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a20693f3e83053dd99499d486412b66777189f1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Изучение методы изменения и представления изменения
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.


В этом разделе рассмотрим методы созданный действий и представления для контроллера фильма. Затем вы добавите настраиваемую страницу поиска.

Запустите приложение и перейдите к `Movies` контроллера путем добавления */Movies* URL-адрес в адресной строке браузера. Наведите указатель мыши на **изменить** ссылку, чтобы увидеть URL-адрес, на которые она ссылается.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Изменить** ссылок, созданных `Html.ActionLink` метод в *Views\Movies\Index.cshtml* представления:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Объект является вспомогательный класс, который предоставляется с помощью свойства на [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) базового класса. `ActionLink` Метод вспомогательного метода упрощает для динамического создания гиперссылки HTML, которые ссылаются на методы действий на контроллерах. Первый аргумент `ActionLink` метод является текст ссылки для подготовки к просмотру (например, `<a>Edit Me</a>`). Второй аргумент — имя вызываемого метода действия. Последний аргумент является [анонимный объект](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , приводит к возникновению ошибки (в данном случае идентификатор 4) данные маршрута.

Созданная ссылка, показанный на предыдущем рисунке — `http://localhost:xxxxx/Movies/Edit/4`. Маршрут по умолчанию (в *приложения\_Start\RouteConfig.cs*) принимает шаблон URL-адреса `{controller}/{action}/{id}`. Таким образом, преобразует ASP.NET `http://localhost:xxxxx/Movies/Edit/4` в запрос на `Edit` метод действия `Movies` контроллера с помощью параметра `ID` равно 4. Изучите следующий код из *приложения\_Start\RouteConfig.cs* файла.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Можно также передать строку запроса с помощью параметров методов действий. Например, URL-адрес `http://localhost:xxxxx/Movies/Edit?ID=4` также передает параметр `ID` от 4 до `Edit` метод действия `Movies` контроллера.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Откройте `Movies` контроллера. Два `Edit` методы действия приведены ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Обратите внимание на второй метод действия `Edit`, которому предшествует атрибут `HttpPost`. Этот атрибут указывает, что перегрузка `Edit` метод может вызываться только для запросов POST. Можно применить `HttpGet` изменение атрибута для первого метода, но это необязательно, так как он используется по умолчанию. (Мы будем называть методы действий, назначенных неявно `HttpGet` атрибута `HttpGet` методов.)

`HttpGet` `Edit` Метод принимает параметр ID фильм, ищет фильм, использующий Entity Framework `Find` метод и возвращает представление изменения выбранного фрагмента. Указывает параметр ID [значение по умолчанию](https://msdn.microsoft.com/library/dd264739.aspx) нуль, если `Edit` метод вызывается без параметров. Если не удается найти фильм, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) возвращается. Если в представлении редактирования создана система формирования шаблонов, она проверяет класс `Movie` и создает код для отображения элементов `<label>` и `<input>` для каждого свойства класса. В следующем примере показано представление изменения, который был создан:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Обратите внимание, как Просмотр шаблона `@model MvcMovie.Models.Movie` инструкции в верхней части файла — это указывает, что представление ожидает модели для представления шаблона типа `Movie`.

Код формирования шаблонов использует несколько *вспомогательные методы* упрощение разметки HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Вспомогательный объект отображает имя поля (&quot;заголовок&quot;, &quot;ReleaseDate&quot;, &quot;жанр&quot;, или &quot;цены &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Вспомогательный отображает HTML- `<input>` элемента. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Вспомогательный объект отображает все сообщения проверки, связанные с этим свойством.

Запустите приложение и перейдите к */Movies* URL-адрес. Щелкните ссылку **Edit** (Изменить). Просмотрите исходный код страницы в окне браузера. Ниже приведен код HTML для элемента form.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Элементы, в HTML- `<form>` элемент которого `action` атрибут имеет значение post для */фильмов/Edit* URL-адрес. Данные формы будут опубликованы на сервере при **изменить** кнопки.

## <a name="processing-the-post-request"></a>Обработка запроса POST

В следующем листинге демонстрируется версия `HttpPost` метода действия `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[Связыватель модели ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) принимает значения из отправленной формы и создает `Movie` объекта, переданного в качестве `movie` параметра. Метод `ModelState.IsValid` проверяет, можно ли использовать переданные в форме данные для изменения (редактирования или обновления) объекта `Movie`. Если данные являются допустимыми, данным фильма сохраняется `Movies` коллекцию `db(MovieDBContext` экземпляра). Новые данные фильма сохраняется в базе данных путем вызова `SaveChanges` метод `MovieDBContext`. После сохранения данных, код выполняет перенаправление пользователю `Index` метод действия `MoviesController` класса, который отображает коллекции фильмов, включая только что внесли изменения.

Если переданные значения не являются допустимыми, они снова отображаются в форме. `Html.ValidationMessageFor` Помощников в *Edit.cshtml* представление шаблона аккуратно отображения соответствующие сообщения об ошибках.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> Поддержка проверки jQuery для языков, кроме английского, используйте запятую (&quot;,&quot;) для десятичной запятой, необходимо включить *globalize.js* и конкретных *cultures/globalize.cultures.js* файл (из [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`. В следующем коде показано изменения в файле Views\Movies\Edit.cshtml для работы с &quot;fr-FR&quot; языка и региональных параметров:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Десятичное поле требуется запятая, а не десятичной запятой. Как временные исправление можно добавить элемент глобализации для проектов корневого файла web.config. В следующем коде показано элемент глобализации с языком и региональными параметрами, задайте для английского (США).

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Все `HttpGet` методы выполните той же модели. Они получают объект фильма (или список объектов, в случае `Index`) и передать модель в представление. `Create` Метод передает пустые фильма объекта представления создания. Все методы, которые создают, редактируют, удаляют или иным образом изменяют данные, делают это в перегрузке метода `HttpPost`. Изменение данных в методе HTTP GET представляет угрозу безопасности, как описано в записи блога post [ASP.NET MVC совет #46 — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Изменение данных в метод GET также нарушает HTTP рекомендации и архитектурное [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) шаблона, который указывает, что запросы GET не должны изменять состояние приложения. Другими словами, операция GET должна выполняться безопасным способом, то есть не иметь побочных эффектов и не изменять существующие данные.

## <a name="adding-a-search-method-and-search-view"></a>Добавление метода поиска и просмотра поиска

В этом разделе вы добавите `SearchIndex` метод действия, который позволяет искать фильмов по жанру или имени. Это будет доступно при использовании */фильмов/SearchIndex* URL-адрес. Запрос отображает форму HTML, содержащий входные элементы, которые пользователь может ввести, чтобы искать фильма. Когда пользователь отправляет форму, метод действия получения поиска значений, переданных пользователем и значения можно использовать для поиска в базе данных.

## <a name="displaying-the-searchindex-form"></a>Отображение формы SearchIndex

Начните с добавления `SearchIndex` метода действия для существующего `MoviesController` класса. Метод возвращает представление, содержащее формы HTML. Ниже приведен код:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Первая строка `SearchIndex` метод создает следующие [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрос, чтобы выбрать фильмы:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Запрос на этом этапе определяется, но еще не было запущено в хранилище данных.

Если `searchString` параметр содержит строку, запрос фильмы изменяется для фильтрации по значению строки поиска, используя следующий код:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Приведенный выше код `s => s.Title` представляет собой [лямбда-выражение](https://msdn.microsoft.com/library/bb397687.aspx). Лямбда-выражения, используемые в основе метода [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрашивает качестве аргументов стандартных методов операторов запроса, таких как [где](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) метод, используемый в приведенном выше коде. LINQ запросы не выполняются при их определении либо изменении путем вызова метода, например `Where` или `OrderBy`. Вместо этого выполнение запроса отложено, это означает, что вычисление выражения откладывается до его реализованных значение фактически итерации или [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) вызывается метод. В `SearchIndex` примера запрос выполняется в режиме SearchIndex. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/library/bb738633.aspx).

Теперь вы можете реализовать `SearchIndex` представление, которое будет отображать формы для пользователя. Щелкните правой кнопкой мыши внутри `SearchIndex` метода и нажмите кнопку **добавить представление**. В **добавить представление** диалоговом окне укажите, что вы собираетесь передать `Movie` объект в шаблоне представления, как и класс модели. В **шаблон формирования** выберите **списка**, нажмите кнопку **добавить**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

При нажатии кнопки **добавить** кнопки, *Views\Movies\SearchIndex.cshtml* создания шаблона представления. Так как вы выбрали **списка** в **шаблон формирования** список, автоматически созданные Visual Studio (формирования шаблонов) некоторую разметку по умолчанию в представлении. Формирование шаблонов создать HTML-формы. Он проверяет `Movie` класса и созданный код для визуализации `<label>` элементы для каждого свойства класса. В следующем списке показана представления создания, который был создан:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Запустите приложение и перейдите к */фильмов/SearchIndex*. Добавьте в URL-адрес строку запроса, например `?searchString=ghost`. Отображаются отфильтрованные фильмы.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Если изменить подпись `SearchIndex` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набор в *Global.asax* файла.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Исходный `SearchIndex` метод выглядит следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Измененный `SearchIndex` метода будет выглядеть следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов. Поэтому теперь вы добавите пользовательского интерфейса для их фильтрации фильмов. Если изменить подпись `SearchIndex` метод для проверки как передавать параметр ID привязкой маршрута, изменить его, чтобы вашей `SearchIndex` метод принимает строковый параметр с именем `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Откройте *Views\Movies\SearchIndex.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте следующий код:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

В следующем примере показано часть *Views\Movies\SearchIndex.cshtml* файл добавлены фильтрации разметкой.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Вспомогательный создает открывающий `<form>` тег. `Html.BeginForm` Вспомогательный вызывает формы post на самого себя, когда пользователь отправляет форму, нажав кнопку **фильтра** кнопки.

Запустите приложение и выполните поиск фильма.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Имеется не `HttpPost` перегрузки `SearchIndex` метод. Это не требуется, поскольку метод не изменяет состояние приложения, просто фильтрации данных.

Можно добавить следующий метод `HttpPost SearchIndex`. В этом случае средство вызова действий будет соответствовать `HttpPost SearchIndex` метода и `HttpPost SearchIndex` метод будет выполняться, как показано на рисунке ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Тем не менее при добавлении этой версии `HttpPost` метода `SearchIndex` существует ограничение на общую реализацию. Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов. Обратите внимание, что URL-адрес для запроса HTTP POST совпадает с URL-адрес для запроса на получение (localhost:xxxxx/фильмов/SearchIndex) — нет поиска сведений в самом URL-АДРЕСЕ. Справа теперь данные строки поиска отправляется на сервер как значения поля формы. Это означает, что вы не можете получить эту информацию поиска для закладки или отправить друзьям в URL-АДРЕСЕ.

Рекомендуется использовать перегрузку `BeginForm` , указывающий, запрос POST должна добавить поиск сведений о URL-адрес и должно быть направлено на версию HttpGet `SearchIndex` метод. Заменить существующий без параметров `BeginForm` метод со следующими:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Теперь при отправке поиска, URL-адрес содержит строку запроса поиска. Поиск также переносится в метод `HttpGet SearchIndex`, даже если у вас определен метод `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Добавление поиска по жанру

Если вы добавили `HttpPost` версии `SearchIndex` метод, сейчас ее удалить.

Далее можно добавить функцию, чтобы позволить пользователям поиск фильмов по жанру. Замените метод `SearchIndex` следующим кодом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Эта версия `SearchIndex` метод принимает дополнительный параметр, а именно `movieGenre`. Первые несколько строк кода, создание `List` объекта, содержащего жанров фильмов из базы данных.

Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех уникальных жанров. (Без `Distinct` модификатор, следует добавить повторяющийся жанров — например, комедия добавляется дважды в нашем примере). Код сохраняет список жанров в `ViewBag` объекта.

В следующем коде показано, как проверить `movieGenre` параметра. Если она не пустая, код далее ограничивает запрос фильмов с целью ограничения фильмы, выбранных для заданного жанра.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Добавление разметки представление SearchIndex для поддержки поиска по жанру

Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\SearchIndex.cshtml* файла, непосредственно перед `TextBox` вспомогательный. Ниже приводится полная разметка.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Запустите приложение и перейдите к */фильмов/SearchIndex*. Попробуйте выполните поиск по жанру, названия фильма и обоим критериям.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

В этом разделе проверяется CRUD методы действий и представления, сформированные платформой. Поиск метода действия и представления, которые позволяют пользователям выполнять поиск по название фильма и жанр созданы. В следующем разделе будут рассмотрены способы Добавление свойства `Movie` модели и как добавить инициализатор, который автоматически создает тестовую базу данных.

>[!div class="step-by-step"]
[Назад](accessing-your-models-data-from-a-controller.md)
[Вперед](adding-a-new-field-to-the-movie-model-and-table.md)
