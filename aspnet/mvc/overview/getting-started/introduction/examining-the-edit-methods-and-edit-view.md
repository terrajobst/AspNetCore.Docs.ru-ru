---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Изучение методов Edit и представления Edit | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 950af5fad99bebc5f9d49f30c2ce1e9e4e5c0f25
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837504"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Изучение методов Edit и представления Edit
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

В этом разделе вы рассмотрите созданный `Edit` методы действий и представления для контроллера movie. Но сначала потребуется короткий интересную вносить поиска лучше даты выпуска. Откройте *Models\Movie.cs* файл и добавьте указанные ниже выделенные строки:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Вы также можете даты язык и региональные параметры конкретного следующим образом:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Пространство имен [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) будет рассмотрено в следующем учебнике. Атрибут [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) определяет отображаемое имя поля (в этом случае "Release Date" вместо "ReleaseDate"). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут указывает тип данных, в данном случае это дата, поэтому сведения о времени, хранящиеся в поле не отображается. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут необходим для ошибки в браузере Chrome, неправильно отображает форматы даты.

Запустите приложение и перейдите к `Movies` контроллера. Наведите указатель мыши на **изменить** ссылку, чтобы просмотреть URL-адрес, на которые она ссылается.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Изменить** ссылку созданное `Html.ActionLink` метод в *Views\Movies\Index.cshtml* представления:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Объект — это вспомогательный объект, который предоставляется с помощью свойства на [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) базового класса. `ActionLink` Метод вспомогательного приложения упрощает процесс динамического создания гиперссылки HTML, к методам действий на контроллерах. Первый аргумент `ActionLink` метод является текст ссылки для подготовки к просмотру (например, `<a>Edit Me</a>`). Вторым аргументом является имя вызываемого метода действия (в этом случае `Edit` действие). Последний аргумент обозначает [анонимный объект](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , создающий данные маршрута (в нашем примере идентификатор 4).

Созданная ссылка, показанный на предыдущем рисунке — `http://localhost:1234/Movies/Edit/4`. Маршрут по умолчанию (в *приложения\_Start\RouteConfig.cs*) принимает шаблон URL-адреса `{controller}/{action}/{id}`. Таким образом, преобразует ASP.NET `http://localhost:1234/Movies/Edit/4` в запрос на `Edit` метод действия `Movies` контроллера с помощью параметра `ID` равным 4. Изучите следующий код из *приложения\_Start\RouteConfig.cs* файла. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) метод используется для маршрутизации HTTP-запросы в соответствующий метод контроллера и действия и указать необязательный параметр ID. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) метод также используется [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) например `ActionLink` для формирования URL-адреса заданы контроллер, метод действия и данные маршрута.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Можно также передать параметрами метода действия, используя строку запроса. Например, URL-адрес `http://localhost:1234/Movies/Edit?ID=3` также передает параметр `ID` 3, `Edit` метод действия `Movies` контроллера.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Откройте `Movies` контроллера. Два `Edit` методы действий, показаны ниже.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Обратите внимание на второй метод действия `Edit`, которому предшествует атрибут `HttpPost`. Этот атрибут указывает, что перегрузка `Edit` метод может вызываться только для запросов POST. Вы можете применить `HttpGet` атрибут к первому изменить метод, но это необязательно, так как он используется по умолчанию. (Я вернусь к методам действий, назначенных неявно `HttpGet` атрибут как `HttpGet` методов.) [Привязать](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) атрибут — другой механизм важный элемент обеспечения безопасности, который предотвращает хакеров чрезмерной передачи данных данные в модель. Свойства необходимо включать только в атрибуте привязки, который вы хотите изменить. Сведения о чрезмерной передачи данных и привязки атрибута в моей [чрезмерную передачу данных Примечание по безопасности](https://go.microsoft.com/fwlink/?LinkId=317598). В случае простой модели, используемые в этом руководстве будет осуществляться привязка все данные в модели. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) атрибут используется для предотвращения подделки запросов и объединен с `@Html.AntiForgeryToken()` в файле представления редактирования (*Views\Movies\Edit.cshtml*), ниже приведен фрагмент:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` Создает маркер защиты от подделки скрытой форме, который должен соответствовать `Edit` метод `Movies` контроллера. Дополнительные сведения о межсайтовых запросов подделки (также известные как XSRF или CSRF) в моем руководстве [защиты от XSRF/CSRF в MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Метод принимает параметр идентификатор фильма, выполняет поиск фильма с помощью Entity Framework `Find` метод и возвращает выбранный фильм в представление редактирования. Если не удается найти фильм, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) возвращается. Если в представлении редактирования создана система формирования шаблонов, она проверяет класс `Movie` и создает код для отображения элементов `<label>` и `<input>` для каждого свойства класса. В следующем примере показано представление редактирования, созданное системой формирования шаблонов visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Обратите внимание на то, каким образом шаблона представления содержится `@model MvcMovie.Models.Movie` инструкция в верхней части файла — это указывает, что в представлении требуется модель представления шаблона типа `Movie`.

В шаблонном коде используется несколько *вспомогательные методы* для оптимизации разметки HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Вспомогательный объект отображает имя поля (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;жанр&quot;, или &quot;цена &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Вспомогательный отображает HTML- `<input>` элемент. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Вспомогательный объект отображает любые сообщения проверки, связанные с этим свойством.

Запустите приложение и перейдите к */Movies* URL-адрес. Щелкните ссылку **Edit** (Изменить). Просмотрите исходный код страницы в окне браузера. Ниже приведен HTML для элемента form.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Элементы являются в HTML- `<form>` элемент которого `action` атрибут имеет значение для публикации */Movies/Edit* URL-адрес. Данные формы будут опубликованы на сервере при **Сохранить** кнопки. Во второй строке отображается скрытый [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) маркером создаваемым `@Html.AntiForgeryToken()` вызова.

## <a name="processing-the-post-request"></a>Обработка запроса POST

В следующем листинге демонстрируется версия `HttpPost` метода действия `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) проверяет атрибут [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) маркером создаваемым `@Html.AntiForgeryToken()` вызвать в представлении.

[Связывателя модели ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) принимает отправленные значения формы и создает `Movie` объект, который передается в качестве `movie` параметра. Метод `ModelState.IsValid` проверяет, можно ли использовать переданные в форме данные для изменения (редактирования или обновления) объекта `Movie`. Если данные являются допустимыми, данные фильма сохраняются `Movies` коллекцию `db(MovieDBContext` экземпляра). Новые данные фильма сохраняются в базу данных путем вызова `SaveChanges` метод `MovieDBContext`. После сохранения данных код перенаправляет пользователя в метод действия `Index` класса `MoviesController`, который отображает коллекцию фильмов с учетом только что внесенных изменений.

Как только проверка на стороне клиента определяет, что значения поля не являются допустимыми, отображается сообщение об ошибке. Если отключить JavaScript, у вас не будет проверка на стороне клиента, но сервер обнаружит переданные значения не являются допустимыми, а значения формы будут отображены повторно с сообщениями об ошибках. Далее в этом руководстве мы рассмотрим проверки более подробно.

`Html.ValidationMessageFor` Вспомогательные функции в *Edit.cshtml* представление шаблона позаботится об отображении соответствующие сообщения об ошибках.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Все `HttpGet` методы следуют одной схеме. Они получают объект фильма (или список объектов, в случае `Index`) и передачи в представление. `Create` Метод передает пустой объект фильма представления создания. Все методы, которые создают, редактируют, удаляют или иным образом изменяют данные, делают это в перегрузке метода `HttpPost`. Изменение данных в методе HTTP GET представляет угрозу безопасности, как описано в записи блога post [ASP.NET MVC совет #46 — не использовать ссылки, удалить, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Изменение данных в метод GET также не соответствует рекомендациям по HTTP и архитектуры [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) шаблона, которое указывает, что запросы GET не должны изменять состояние приложения. Другими словами, операция GET должна выполняться безопасным способом, то есть не иметь побочных эффектов и не изменять существующие данные.

Если вы используете компьютер английского (США), можно пропустить этот раздел и перейдите к следующему руководству. Можно загрузить версию этого учебника, Globalize [здесь](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Отличная два часть руководства по интернационализации, см. в разделе [Надим в ASP.NET MVC 5 интернационализации](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> Поддержка проверки jQuery для английского, используйте запятую (&quot;,&quot;) для десятичного разделителя и форматов даты неанглийские США, необходимо включить *globalize.js* и конкретными  *cultures/globalize.cultures.js* файла (из [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`. Проверка локализованной jQuery можно получить из NuGet. (Не устанавливайте Globalize при использовании английского языка.)


1. Из **средства** меню выберите **диспетчера пакетов NuGetLibrary**, а затем нажмите кнопку **управление пакетами NuGet для решения**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. На левой панели выберите <strong>Обзор *.</strong>* (См. на рисунке ниже).
3. В поле ввода введите * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Выберите `jQuery.Validation.Globalize`, выберите `MvcMovie` и нажмите кнопку **установить**. *Scripts\jquery.globalize\globalize.js* файл будет добавлен в проект. *Scripts\jquery.globalize\cultures\* папка будет содержать большое количество файлов JavaScript языка и региональных параметров. Обратите внимание, что может занять 5 минут, чтобы установить этот пакет.

   В следующем коде показано изменения в файл Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Во избежание повторять этот код в каждое представление редактирования, можно переместить его в файл макета. Для оптимизации загружаемом файле сценария, см. в разделе my руководстве [объединение и Минификация](../../performance/bundling-and-minification.md).

Дополнительные сведения см. в разделе [ASP.NET MVC 3 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx) и [Интернационализация 3 MVC для ASP.NET — часть 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Как временное решение Если не удается получить проверки, работа в языковой стандарт, вы можете принудительно компьютера для использования английского (США), или вы можете отключить JavaScript в браузере. Чтобы принудительно использовать английский (США), можно добавить элемент глобализации в корневой каталог проектов *web.config* файл. В следующем коде показано элементе globalization с языком и региональными параметрами, задайте для английского (США).

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> В следующем учебном курсе мы реализуем функции поиска.

> [!div class="step-by-step"]
> [Назад](accessing-your-models-data-from-a-controller.md)
> [Вперед](adding-search.md)
