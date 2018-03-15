---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: "Изучение методы изменения и представления изменения (C#) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 137254be69344f31e65e1b4d1318a107ff9d6d47
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Изучение методы изменения и представления изменения (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.
> 
> 
> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходного кода C# доступна по следующему адресу. [Загрузка версии C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если используется Visual Basic, перейдите в [Visual Basic-версии](../vb/intro-to-aspnet-mvc-3.md) этого учебника.


В этом разделе рассмотрим методы созданный действий и представления для контроллера фильма. Затем вы добавите настраиваемую страницу поиска.

Запустите приложение и перейдите к `Movies` контроллера путем добавления */Movies* URL-адрес в адресной строке браузера. Наведите указатель мыши на **изменить** ссылку, чтобы увидеть URL-адрес, на которые она ссылается.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Изменить** ссылок, созданных `Html.ActionLink` метод в *Views\Movies\Index.cshtml* представления:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

`Html` Объект является вспомогательный класс, который предоставляется с помощью свойства на `WebViewPage` базового класса. `ActionLink` Метод вспомогательного метода упрощает для динамического создания гиперссылки HTML, которые ссылаются на методы действий на контроллерах. Первый аргумент `ActionLink` метод является текст ссылки для подготовки к просмотру (например, `<a>Edit Me</a>`). Второй аргумент — имя вызываемого метода действия. Последний аргумент является [анонимный объект](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , приводит к возникновению ошибки (в данном случае идентификатор 4) данные маршрута.

Созданная ссылка, показанный на предыдущем рисунке — `http://localhost:xxxxx/Movies/Edit/4`. Маршрут по умолчанию принимает шаблон URL-адреса `{controller}/{action}/{id}`. Таким образом, преобразует ASP.NET `http://localhost:xxxxx/Movies/Edit/4` в запрос на `Edit` метод действия `Movies` контроллера с помощью параметра `ID` равно 4.

Можно также передать строку запроса с помощью параметров методов действий. Например, URL-адрес `http://localhost:xxxxx/Movies/Edit?ID=4` также передает параметр `ID` от 4 до `Edit` метод действия `Movies` контроллера.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Откройте `Movies` контроллера. Два `Edit` методы действия приведены ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Обратите внимание на второй метод действия `Edit`, которому предшествует атрибут `HttpPost`. Этот атрибут указывает, что перегрузка `Edit` метод может вызываться только для запросов POST. Можно применить `HttpGet` изменение атрибута для первого метода, но это необязательно, так как он используется по умолчанию. (Мы будем называть методы действий, назначенных неявно `HttpGet` атрибута `HttpGet` методов.)

`HttpGet` `Edit` Метод принимает параметр ID фильм, ищет фильм, использующий Entity Framework `Find` метод и возвращает представление изменения выбранного фрагмента. Если в представлении редактирования создана система формирования шаблонов, она проверяет класс `Movie` и создает код для отображения элементов `<label>` и `<input>` для каждого свойства класса. В следующем примере показано представление изменения, который был создан:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Обратите внимание, как Просмотр шаблона `@model MvcMovie.Models.Movie` инструкции в верхней части файла — это указывает, что представление ожидает модели для представления шаблона типа `Movie`.

Код формирования шаблонов использует несколько *вспомогательные методы* упрощение разметки HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Вспомогательный объект отображает имя поля («Title», «ReleaseDate», «Жанр» или «Price»). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Вспомогательный объект отображает HTML `<input>` элемента. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Вспомогательный объект отображает все сообщения проверки, связанные с этим свойством.

Запустите приложение и перейдите к */Movies* URL-адрес. Щелкните ссылку **Edit** (Изменить). Просмотрите исходный код страницы в окне браузера. На странице HTML выглядит как следующий пример. (Меню разметки был исключен для ясности).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

`<input>` Элементы, в HTML- `<form>` элемент которого `action` атрибут имеет значение post для */фильмов/Edit* URL-адрес. Данные формы будут опубликованы на сервере при **изменить** кнопки.

## <a name="processing-the-post-request"></a>Обработка запроса POST

В следующем листинге демонстрируется версия `HttpPost` метода действия `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

Связыватель модели ASP.NET framework принимает значения из отправленной формы и создает `Movie` объекта, переданного в качестве `movie` параметра. `ModelState.IsValid` Проверка в коде подтверждает, что данные, переданные в форме может использоваться для изменения `Movie` объекта. Если данные являются допустимыми, код сохраняет данные фильма с `Movies` коллекцию `MovieDBContext` экземпляра. Затем код сохраняет новые данные фильма в базу данных, вызвав `SaveChanges` метод `MovieDBContext`, который сохраняет изменения в базу данных. После сохранения данных, код выполняет перенаправление пользователю `Index` метод действия `MoviesController` класс, который вызывает обновленный фильма для отображения в список фильмов.

Если переданные значения не являются допустимыми, они снова отображаются в форме. `Html.ValidationMessageFor` Помощников в *Edit.cshtml* представление шаблона аккуратно отображения соответствующие сообщения об ошибках.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Примечание о локалях** Если обычно вы работаете с языковым стандартом, отличный от английского, см. раздел [поддержкой ASP.NET MVC 3 проверки с языковыми стандартами, отличные от английского.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Сделать более надежный метод редактирования

`HttpGet` `Edit` Метода, создаваемая системой формирования шаблонов не проверяет допустимость идентификатор, который передается в него. Если пользователь удаляет сегмент идентификатора в URL-адресе (`http://localhost:xxxxx/Movies/Edit`), отображается следующая ошибка:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Пользователь также может передать идентификатор, который не существует в базе данных, таких как `http://localhost:xxxxx/Movies/Edit/1234`. Можно сделать два изменения `HttpGet` `Edit` метода действия для устранения этой проблемы. Во-первых, изменить `ID` параметр иметь нулевое значение по умолчанию, если идентификатор не передается явным образом. Можно также убедиться, что `Find` фильма фактически найти метод перед возвращением объекта фильма шаблона представления. Обновленный `Edit` метода показан ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

При обнаружении не фильма `HttpNotFound` вызывается метод.

Все `HttpGet` методы выполните той же модели. Они получают объект фильма (или список объектов, в случае `Index`) и передать модель в представление. `Create` Метод передает пустые фильма объекта представления создания. Все методы, которые создают, редактируют, удаляют или иным образом изменяют данные, делают это в перегрузке метода `HttpPost`. Изменение данных в методе HTTP GET представляет угрозу безопасности, как описано в записи блога post [ASP.NET MVC совет #46 — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Изменение данных в метод GET также приводит к нарушению HTTP рекомендации и шаблон архитектуры REST, который указывает, что запросы GET не должны изменять состояние приложения. Другими словами выполняя операцию получения следует безопасной работы, которая не имеет побочных эффектов.

## <a name="adding-a-search-method-and-search-view"></a>Добавление метода поиска и просмотра поиска

В этом разделе вы добавите `SearchIndex` метод действия, который позволяет искать фильмов по жанру или имени. Это будет доступно при использовании */фильмов/SearchIndex* URL-адрес. Запрос отображает HTML-форму, содержащую входные элементы, которые можно заполнить пользователя, чтобы искать фильма. Когда пользователь отправляет форму, метод действия получения поиска значений, переданных пользователем и значения можно использовать для поиска в базе данных.

## <a name="displaying-the-searchindex-form"></a>Отображение формы SearchIndex

Начните с добавления `SearchIndex` метода действия для существующего `MoviesController` класса. Метод возвращает представление, содержащее формы HTML. Ниже приведен код:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

Первая строка `SearchIndex` метод создает следующие [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) запрос, чтобы выбрать фильмы:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

Запрос на этом этапе определяется, но еще не было запущено в хранилище данных.

Если `searchString` параметр содержит строку, запрос фильмы изменяется для фильтрации по значению строки поиска, используя следующий код:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

LINQ запросы не выполняются при их определении либо изменении путем вызова метода, например `Where` или `OrderBy`. Вместо этого выполнение запроса отложено, это означает, что вычисление выражения откладывается до его реализованных значение фактически итерации или [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) вызывается метод. В `SearchIndex` примера запрос выполняется в режиме SearchIndex. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://msdn.microsoft.com/library/bb738633.aspx).

Теперь вы можете реализовать `SearchIndex` представление, которое будет отображать формы для пользователя. Щелкните правой кнопкой мыши внутри `SearchIndex` метода и нажмите кнопку **добавить представление**. В **добавить представление** диалоговом окне укажите, что вы собираетесь передать `Movie` объект в шаблоне представления, как и класс модели. В **шаблон формирования** выберите **списка**, нажмите кнопку **добавить**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

При нажатии кнопки **добавить** кнопки, *Views\Movies\SearchIndex.cshtml* создания шаблона представления. Так как вы выбрали **списка** в **шаблон формирования** список, автоматически созданные Visual Web Developer (формирования шаблонов) содержимое по умолчанию в представлении. Формирование шаблонов создать HTML-формы. Он проверяет `Movie` класса и созданный код для визуализации `<label>` элементы для каждого свойства класса. В следующем списке показана представления создания, который был создан:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Запустите приложение и перейдите к */фильмов/SearchIndex*. Добавьте в URL-адрес строку запроса, например `?searchString=ghost`. Отображаются отфильтрованные фильмы.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Если изменить подпись `SearchIndex` методу иметь параметр с именем `id`, `id` параметр будет соответствовать `{id}` заполнитель для значения по умолчанию направляет набор в *Global.asax* файла.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Измененный `SearchIndex` метода будет выглядеть следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов. Поэтому теперь вы добавите пользовательского интерфейса для их фильтрации фильмов. Если изменить подпись `SearchIndex` метод для проверки как передавать параметр ID привязкой маршрута, изменить его, чтобы вашей `SearchIndex` метод принимает строковый параметр с именем `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Откройте *Views\Movies\SearchIndex.cshtml* файла и сразу же после `@Html.ActionLink("Create New", "Create")`, добавьте следующий код:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

В следующем примере показано часть *Views\Movies\SearchIndex.cshtml* файл добавлены фильтрации разметкой.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

`Html.BeginForm` Вспомогательный создает открывающий `<form>` тег. `Html.BeginForm` Вспомогательный вызывает формы post на самого себя, когда пользователь отправляет форму, нажав кнопку **фильтра** кнопки.

Запустите приложение и выполните поиск фильма.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Имеется не `HttpPost` перегрузки `SearchIndex` метод. Это не требуется, поскольку метод не изменяет состояние приложения, просто фильтрации данных.

Можно добавить следующий метод `HttpPost SearchIndex`. В этом случае средство вызова действий будет соответствовать `HttpPost SearchIndex` метода и `HttpPost SearchIndex` метод будет выполняться, как показано на рисунке ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Тем не менее при добавлении этой версии `HttpPost` метода `SearchIndex` существует ограничение на общую реализацию. Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов. Обратите внимание, что URL-адрес для запроса HTTP POST совпадает с URL-адрес для запроса на получение (localhost:xxxxx/фильмов/SearchIndex) — нет поиска сведений в самом URL-АДРЕСЕ. Справа теперь данные строки поиска отправляется на сервер как значения поля формы. Это означает, что вы не можете получить эту информацию поиска для закладки или отправить друзьям в URL-АДРЕСЕ.

Рекомендуется использовать перегрузку `BeginForm` , указывает, что запрос POST следует добавить сведения о поиска URL-адрес, и это должно быть направлено в версии HttpGet `SearchIndex` метод. Заменить существующий без параметров `BeginForm` метод со следующими:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

Теперь при отправке поиска, URL-адрес содержит строку запроса поиска. Поиск также переносится в метод `HttpGet SearchIndex`, даже если у вас определен метод `HttpPost SearchIndex`.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Добавление поиска по жанру

Если вы добавили `HttpPost` версии `SearchIndex` метод, сейчас ее удалить.

Далее можно добавить функцию, чтобы позволить пользователям поиск фильмов по жанру. Замените метод `SearchIndex` следующим кодом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Эта версия `SearchIndex` метод принимает дополнительный параметр, а именно `movieGenre`. Первые несколько строк кода, создание `List` объекта, содержащего жанров фильмов из базы данных.

Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

Код использует `AddRange` метод универсального `List` коллекции для добавления в список всех уникальных жанров. (Без `Distinct` модификатор, следует добавить повторяющийся жанров — например, комедия добавляется дважды в нашем примере). Код сохраняет список жанров в `ViewBag` объекта.

В следующем коде показано, как проверить `movieGenre` параметра. Если она не пустая код далее ограничивает запрос фильмов с целью ограничения фильмы, выбранных для заданного жанра.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Добавление разметки представление SearchIndex для поддержки поиска по жанру

Добавить `Html.DropDownList` вспомогательный метод для *Views\Movies\SearchIndex.cshtml* файла, непосредственно перед `TextBox` вспомогательный. Ниже приводится полная разметка.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Запустите приложение и перейдите к */фильмов/SearchIndex*. Попробуйте выполните поиск по жанру, названия фильма и обоим критериям.

В этом разделе проверяется CRUD методы действий и представления, сформированные платформой. Поиск метода действия и представления, которые позволяют пользователям выполнять поиск по название фильма и жанр созданы. В следующем разделе будут рассмотрены способы Добавление свойства `Movie` модели и как добавить инициализатор, который автоматически создает тестовую базу данных.

>[!div class="step-by-step"]
[Назад](accessing-your-models-data-from-a-controller.md)
[Вперед](adding-a-new-field.md)
