---
title: "Вспомогательная функция тега привязки"
author: pkellner
description: "Обнаруживайте атрибуты вспомогательной функции тега привязки ASP.NET Core и роль, которую играет каждый атрибут в расширении поведения тега привязки HTML."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a>Вспомогательная функция тега привязки

Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) и [Скотт Эдди](https://github.com/scottaddie) (Scott Addie).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

[Вспомогательная функция тега привязки](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) повышает эффективность стандартного тега привязки HTML (`<a ... ></a>`) путем добавления новых атрибутов. Как правило, все имена атрибутов начинаются с `asp-`. Отображаемое значение атрибута `href` элемента привязки определяется значениями атрибутов `asp-`.

В примерах в этом документе используется *SpeakerController*

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Ниже перечислены атрибуты `asp-`.

## <a name="asp-controller"></a>asp-controller

Атрибут [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) назначает контроллер, используемый для создания URL-адреса. Следующий элемент перечисляет всех говорящих:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

Созданный HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Если атрибут `asp-controller` указан, а атрибут `asp-action` — нет, действием контроллера, связанным с выполняющимся представлением, будет заданное по умолчанию значение `asp-action`. Если `asp-action` исключается из предыдущего исправления, а в представлении *Index* (*/Home*) *HomeController* используется вспомогательная функция тега привязки, создается следующий код HTML:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

Значение атрибута [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) представляет имя действия контроллера, включенное в созданный атрибут `href`. Следующий элемент задает созданное значение атрибута `href` на странице динамика оценок:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

Созданный HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Если атрибут `asp-controller` не указан, будет использоваться контроллер по умолчанию, вызывающий представление при выполнении текущего представления.

Если атрибут `asp-action` имеет значение `Index`, действия не добавляются к URL-адресу и вызывается метод `Index` по умолчанию. В контроллере, на который есть ссылка в `asp-controller`, должно существовать указанное действие (или заданное по умолчанию).

## <a name="asp-route-value"></a>asp-route-{value}

Атрибут [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) включает подстановочный префикс маршрута. Любое значение, подставляемое вместо `{value}`, рассматривается как потенциальный параметр маршрута. Если маршрут по умолчанию не найден, этот префикс маршрута будет добавлен к созданному атрибуту `href` в виде параметра запроса и значения. В противном случае он будет заменен в шаблоне маршрута.

Рассмотрим следующее действие контроллера:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

С шаблоном маршрута по умолчанию, определенным в *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

Представление MVC использует модель, предоставляемую действием:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

Заполнитель `{id?}` маршрута по умолчанию соответствует требуемому. Созданный HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Предположим, префикс маршрута не входит в состав соответствующего шаблона маршрутизации, как в случае со следующим представлением MVC:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Следующий код HTML будет создан, так как элемент `speakerid` не найден в соответствующем маршруте:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Если атрибут `asp-controller` или `asp-action` не указан, применяется та же обработка по умолчанию, что и в атрибуте `asp-route`.

## <a name="asp-route"></a>asp-route

Атрибут [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) используется для связывания URL-адреса непосредственно с именованным маршрутом. С помощью [атрибутов маршрутизации](xref:mvc/controllers/routing#attribute-routing) маршруту можно присвоить имя, как показано в классе `SpeakerController` и используется в его действии `Evaluations`:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

В следующем элементе атрибут `asp-route` ссылается на именованный маршрут:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Вспомогательная функция тега привязки создает маршрут непосредственно к этому методу контроллера, используя */Speaker/Evaluations* URL-адреса. Созданный HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Если наряду с атрибутом `asp-route` указаны атрибут `asp-controller` или `asp-action`, созданный маршрут может отличаться от ожидаемого. Во избежание конфликта маршрута `asp-route` нельзя использовать вместе с атрибутами `asp-controller` или `asp-action`.

## <a name="asp-all-route-data"></a>asp-all-route-data

Атрибут [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) поддерживает создание словаря пар "ключ-значение". Ключ является именем параметра, а значение — значением параметра.

В следующем примере словарь инициализируется и передается в представление Razor. Кроме того, данные могут быть переданы с помощью модели.

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Предыдущий код вызывает следующий код HTML:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

Словарь `asp-all-route-data` предназначен для создания строки запроса, соответствующей требованиям перегруженного действия `Evaluations`:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

Если ключи в словаре совпадают с параметрами маршрута, эти значения будут соответствующим образом заменены в маршруте. В качестве параметров запроса будут созданы другие несовпадающие значения.

## <a name="asp-fragment"></a>asp-fragment

Атрибут [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) определяет фрагмент URL-адреса, добавляемый к URL-адресу. Вспомогательная функция тега привязки добавляет символ решетки (#). Рассмотрим следующий элемент:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Созданный HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Хэштеги полезны при создании приложений на стороне клиента. Например, их можно использовать для простоты пометки и поиска на языке JavaScript.

## <a name="asp-area"></a>asp-area

Атрибут [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) указывает имя области, используемое платформой для определения соответствующего маршрута. Ниже приведен пример того, как атрибут области приводит к повторному сопоставлению маршрутов. При установке для атрибута `asp-area` значения Blogs перед маршрутами связанных контроллеров и представлений для этого тега привязки добавляется каталог *Areas/Blogs*.

* **<Имя проекта\>**
  * **wwwroot**
  * **Области**
    * **Блоги**
      * **Контроллеры**
        * *HomeController.cs*
      * **Представления**
        * **Корневая папка**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Контроллеры**

С учетом предыдущей иерархии каталогов элемент для ссылки на файл *AboutBlog.cshtml* будет таким:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

Созданный HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Чтобы в приложении MVC использовались области, в шаблон маршрута необходимо включить ссылку на область, если она существует. Этот шаблон представлен вторым параметром вызова метода `routes.MapRoute` в *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

Атрибут [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) предназначен для указания протокола (например, `https`) в URL-адресе. Пример:

[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]

Созданный HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Именем узла в примере является localhost, но при создании URL-адреса вспомогательная функция тега привязки использует общедоступный домен веб-сайта.

## <a name="asp-host"></a>asp-host

Атрибут [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) предназначен для определения имени узла в URL-адресе. Пример:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

Созданный HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

Атрибут [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) используется со страницами Razor Pages. Используйте его для определения значения атрибута `href` тега привязки для определенной страницы. Чтобы создать URL-адрес, перед именем страницы следует ввести символ косой черты (/).

Следующий пример указывает на страницу Razor Pages участника:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

Созданный HTML:

```html
<a href="/Attendee">All Attendees</a>
```

Атрибут `asp-page` является взаимоисключающим с атрибутами `asp-route`, `asp-controller` и `asp-action`. Тем не менее `asp-page` можно использовать с `asp-route-{value}` для управления маршрутизацией, как показано в следующем элементе:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Созданный HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

Атрибут [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) используется со страницами Razor Pages. Он предназначен для связывания с обработчиками определенных страниц.

Рассмотрим следующий обработчик страниц:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Связанный элемент модели страницы ссылается на обработчик страниц `OnGetProfile`. Обратите внимание, что префикс `On<Verb>` имени метода обработчика страниц опущен в значении атрибута `asp-page-handler`. В случае с асинхронным методом суффикс `Async` был бы также опущен.

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Созданный HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Области](xref:mvc/controllers/areas)
* [Общие сведения о Razor Pages](xref:mvc/razor-pages/index)
