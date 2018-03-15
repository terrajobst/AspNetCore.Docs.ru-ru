---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "Изучение методы изменения и представления изменения | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Изучение методы изменения и представления изменения
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

В этом разделе вы изучим созданный `Edit` методы действий и представления для контроллера фильма. Но сначала потребуется короткий переадресация вносить улучшения отображения даты выпуска. Откройте *Models\Movie.cs* файл и добавьте выделенные строки, показано ниже:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Вы также можете культуры даты конкретных следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Пространство имен [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) будет рассмотрено в следующем учебнике. Атрибут [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) определяет отображаемое имя поля (в этом случае "Release Date" вместо "ReleaseDate"). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут задает тип данных, в данном случае это дата, поэтому сведения о времени, содержащегося в поле не отображается. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут необходим для ошибок в браузере Chrome, который неправильно отображает форматы даты.

Запустите приложение и перейдите к `Movies` контроллера. Наведите указатель мыши на **изменить** ссылку, чтобы увидеть URL-адрес, на которые она ссылается.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Изменить** ссылок, созданных `Html.ActionLink` метод в *Views\Movies\Index.cshtml* представления:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Объект является вспомогательный класс, который предоставляется с помощью свойства на [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) базового класса. `ActionLink` Метод вспомогательного метода упрощает для динамического создания гиперссылки HTML, которые ссылаются на методы действий на контроллерах. Первый аргумент `ActionLink` метод является текст ссылки для подготовки к просмотру (например, `<a>Edit Me</a>`). Вторым аргументом является имя метода действия для вызова (в этом случае `Edit` действия). Последний аргумент является [анонимный объект](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , приводит к возникновению ошибки (в данном случае идентификатор 4) данные маршрута.

Созданная ссылка, показанный на предыдущем рисунке — `http://localhost:1234/Movies/Edit/4`. Маршрут по умолчанию (в *приложения\_Start\RouteConfig.cs*) принимает шаблон URL-адреса `{controller}/{action}/{id}`. Таким образом, преобразует ASP.NET `http://localhost:1234/Movies/Edit/4` в запрос на `Edit` метод действия `Movies` контроллера с помощью параметра `ID` равно 4. Изучите следующий код из *приложения\_Start\RouteConfig.cs* файла. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) метод используется для маршрутизации запросов HTTP, соответствующий метод контроллера и действия и предоставляет дополнительный параметр ID. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) также используется метод [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) например `ActionLink` для создания URL-адресов, заданному контроллеру, метод действия и все данные о маршруте.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Можно также передать строку запроса с помощью параметров методов действий. Например, URL-адрес `http://localhost:1234/Movies/Edit?ID=3` также передает параметр `ID` 3, `Edit` метод действия `Movies` контроллера.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Откройте `Movies` контроллера. Два `Edit` методы действия приведены ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Обратите внимание на второй метод действия `Edit`, которому предшествует атрибут `HttpPost`. Этот атрибут указывает, что перегрузка `Edit` метод может вызываться только для запросов POST. Можно применить `HttpGet` изменение атрибута для первого метода, но это необязательно, так как он используется по умолчанию. (Мы будем называть методы действий, назначенных неявно `HttpGet` атрибута `HttpGet` методов.) [Привязки](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) атрибута является один механизм безопасности, который предотвращает перезапись учетных данных в модель хакеров. Свойства должна содержать только в атрибут привязки, который требуется изменить. Вы можете прочесть об overposting и атрибутом привязки в моей [оверпостинга Примечание по безопасности](https://go.microsoft.com/fwlink/?LinkId=317598). В простой модели, используемые в этом учебнике будет осуществляться привязка все данные в модели. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) атрибут используется для предотвращения подделки запросов и объединен с `@Html.AntiForgeryToken()` в файле представление редактирования (*Views\Movies\Edit.cshtml*), ниже показан фрагмент:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` Создает маркер защиты от подделки скрытые формы, должны совпадать в `Edit` метод `Movies` контроллера. Вы можете прочитать больше о межсайтовых запросов подделки (также известный как XSRF или CSRF) в моей учебника [защиты от XSRF-CSRF в MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Метод принимает параметр ID фильм, ищет фильм, использующий Entity Framework `Find` метод и возвращает представление изменения выбранного фрагмента. Если не удается найти фильм, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) возвращается. Если в представлении редактирования создана система формирования шаблонов, она проверяет класс `Movie` и создает код для отображения элементов `<label>` и `<input>` для каждого свойства класса. В следующем примере показано представление изменения, созданный системой формирование шаблонов visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Обратите внимание, как Просмотр шаблона `@model MvcMovie.Models.Movie` инструкции в верхней части файла — это указывает, что представление ожидает модели для представления шаблона типа `Movie`.

Код формирования шаблонов использует несколько *вспомогательные методы* упрощение разметки HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Вспомогательный объект отображает имя поля (&quot;заголовок&quot;, &quot;ReleaseDate&quot;, &quot;жанр&quot;, или &quot;цены &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Вспомогательный отображает HTML- `<input>` элемента. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Вспомогательный объект отображает все сообщения проверки, связанные с этим свойством.

Запустите приложение и перейдите к */Movies* URL-адрес. Щелкните ссылку **Edit** (Изменить). Просмотрите исходный код страницы в окне браузера. Ниже приведен код HTML для элемента form.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Элементы, в HTML- `<form>` элемент которого `action` атрибут имеет значение post для */фильмов/Edit* URL-адрес. Данные формы будут опубликованы на сервере при **Сохранить** кнопки. Вторая показывает скрытого [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) токена, созданные `@Html.AntiForgeryToken()` вызова.

## <a name="processing-the-post-request"></a>Обработка запроса POST

В следующем листинге демонстрируется версия `HttpPost` метода действия `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) проверяет атрибут [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) токена, созданные `@Html.AntiForgeryToken()` вызвать в представлении.

[Связыватель модели ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) принимает значения из отправленной формы и создает `Movie` объекта, переданного в качестве `movie` параметра. Метод `ModelState.IsValid` проверяет, можно ли использовать переданные в форме данные для изменения (редактирования или обновления) объекта `Movie`. Если данные являются допустимыми, данным фильма сохраняется `Movies` коллекцию `db(MovieDBContext` экземпляра). Новые данные фильма сохраняется в базе данных путем вызова `SaveChanges` метод `MovieDBContext`. После сохранения данных код перенаправляет пользователя в метод действия `Index` класса `MoviesController`, который отображает коллекцию фильмов с учетом только что внесенных изменений.

Сразу после проверки на стороне клиента определяет, что не допускаются значения поля, отображается сообщение об ошибке. При отключении JavaScript, вы не будете иметь проверки на стороне клиента, но сервер определит, переданные значения не допускаются, и значения формы будет повторно с сообщениями об ошибках. Далее в этом учебнике рассматриваются проверки более подробно.

`Html.ValidationMessageFor` Помощников в *Edit.cshtml* представление шаблона аккуратно отображения соответствующие сообщения об ошибках.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Все `HttpGet` методы выполните той же модели. Они получают объект фильма (или список объектов, в случае `Index`) и передать модель в представление. `Create` Метод передает пустые фильма объекта представления создания. Все методы, которые создают, редактируют, удаляют или иным образом изменяют данные, делают это в перегрузке метода `HttpPost`. Изменение данных в методе HTTP GET представляет угрозу безопасности, как описано в записи блога post [ASP.NET MVC совет #46 — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Изменение данных в метод GET также нарушает HTTP рекомендации и архитектурное [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) шаблона, который указывает, что запросы GET не должны изменять состояние приложения. Другими словами, операция GET должна выполняться безопасным способом, то есть не иметь побочных эффектов и не изменять существующие данные.

При использовании компьютера английский (США), можно пропустить этот раздел и перейдите на следующем уроке. Можно загрузить версию этого учебника Globalize [здесь](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Отлично двухкомпонентные учебник по интернационализации, см. [Надим в ASP.NET MVC 5 интернационализации](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> Поддержка проверки jQuery для языков, кроме английского, используйте запятую (&quot;,&quot;) для десятичной запятой и форматы дат локализованной США, необходимо включить *globalize.js* и конкретных  *cultures/globalize.cultures.js* файла (из [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`. Проверка локализованной jQuery можно получить из NuGet. (Не устанавливайте Globalize при использовании английского языка.)


1. Из **средства** меню **NuGetLibrary диспетчера пакетов**, а затем нажмите кнопку **управление пакетами NuGet для решения**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. На левой панели выберите **Обзор*. *** (см. на рисунке ниже).
3. В поле ввода введите * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Выберите `jQuery.Validation.Globalize`, выберите `MvcMovie` и нажмите кнопку **установить**. *Scripts\jquery.globalize\globalize.js* файл будет добавлен в проект. *Scripts\jquery.globalize\cultures\* папка будет содержать большое количество файлов JavaScript языка и региональных параметров. Обратите внимание, что может потребоваться пять минут для установки этого пакета.

 В следующем коде показано изменения в файле Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Во избежание повторения этого кода в представление каждого изменения, можно переместить в файл макета. Чтобы оптимизировать загрузки скрипта, см. Мои учебнике [объединении и Минификация](../../performance/bundling-and-minification.md).

Дополнительные сведения см. [ASP.NET MVC 3 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx) и [MVC 3 ASP.NET Интернационализация — часть 2 (обновление NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Как временные исправление Если не удается получить проверки при работе в региональных настроек, можно заставить компьютер на использование английского языка или можно отключить JavaScript в браузере. Чтобы принудительно использовать английский (США), можно добавить элемент глобализации к корню проекты *web.config* файла. В следующем коде показано элемент глобализации с языком и региональными параметрами, задайте для английского (США).

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> В следующем уроке мы реализуется функция поиска.

>[!div class="step-by-step"]
[Назад](accessing-your-models-data-from-a-controller.md)
[Вперед](adding-search.md)
