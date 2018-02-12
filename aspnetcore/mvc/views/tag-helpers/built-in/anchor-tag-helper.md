---
title: "Вспомогательная функция тега привязки в ASP.NET Core MVC"
author: pkellner
description: "Сведения о работе со вспомогательной функцией тега привязки"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>Вспомогательная функция тега привязки

Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) 

Вспомогательная функция тега привязки повышает эффективность тега привязки HTML (`<a ... ></a>`) путем добавления новых атрибутов. Ссылка (в теге `href`) создается с использованием новых атрибутов. Этот URL-адрес может включать в себя необязательный протокол, такой как https.

В примерах в этом документе используется контроллер говорящего.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Атрибуты вспомогательной функция тега привязки

### <a name="asp-controller"></a>asp-controller

`asp-controller` используется для связи контроллера, который будет применяться для создания URL-адреса. Указанные контроллеры должны существовать в текущем проекте. В следующем коде перечисляются все говорящие: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Созданная разметка будет иметь следующий вид:

```html
<a href="/Speaker">All Speakers</a>
```

Если атрибут `asp-controller` указан, а атрибут `asp-action` — нет, методом контроллера по умолчанию текущего представления будет заданный по умолчанию атрибут `asp-action`. То есть если в приведенном выше примере опускается атрибут `asp-action`, а вспомогательная функция тега привязки создается из представления *HomeController* `Index` (**/Home**), полученная разметка будет иметь следующий вид:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` — это имя метода действия в контроллере, которое будет включено в созданный `href`. Например, следующий код задает созданный `href` для указания на страницу сведений о говорящем:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Созданная разметка будет иметь следующий вид:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Если атрибут `asp-controller` не указан, будет использоваться контроллер по умолчанию, вызывающий представление при выполнении текущего представления.  
 
Если атрибут `asp-action` имеет значение `Index`, никакое действие не добавляется к URL-адресу и вызывается метод `Index` по умолчанию. В контроллере, на который есть ссылка в `asp-controller`, должно существовать указанное действие (или заданное по умолчанию).

### <a name="asp-page"></a>asp-page

Атрибут `asp-page` в теге привязки используется для задания его URL-адреса, указывающего на определенную страницу. Чтобы создать URL-адрес, перед именем страницы следует ввести символ косой черты "/". В примере ниже URL-адрес указывает на страницу "Speaker" (Говорящий) в текущем каталоге.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

Атрибут `asp-page` в приведенном выше примере отображает выходные данные в формате HTML в представлении, аналогичном следующему фрагменту кода:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

Атрибут `asp-page` является взаимоисключающим с атрибутами `asp-route`, `asp-controller` и `asp-action`. Однако `asp-page` можно использовать с `asp-route-id` для управления маршрутизацией, как показано в следующем примере кода:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` формирует следующие результаты:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Для использования атрибута `asp-page` на страницах Razor Pages URL-адрес должен быть относительным путем, например `"./Speaker"`. Относительные пути в атрибуте `asp-page` недоступны в представлениях MVC. Вместо этого в представлениях MVC следует использовать синтаксис "/".

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` является префиксом маршрута. Любое значение, помещенное после дефиса в конце, рассматривается как потенциальный параметр маршрута. Если маршрут по умолчанию не найден, этот префикс маршрута будет добавлен к созданному href в виде параметра запроса и значения. В противном случае он будет заменен в шаблоне маршрута.

Предположим, что у вас есть метод контроллера, определенный следующим образом:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

И есть шаблон маршрута по умолчанию, определенный в файле *Startup.cs* следующим образом:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

Файл **cshtml**, содержащий вспомогательную функцию тега привязки, необходимую для использования параметра модели **speaker**, переданного из контроллера в представление, имеет следующий вид:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Так как в маршруте по умолчанию был найден **id**, созданный HTML будет иметь следующий вид:

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Если префикс маршрута не входит в состав обнаруженного шаблона маршрутизации, как, например, в случае со следующим файлом **cshtml**:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Так как в соответствующем маршруте не был найден **speakerid**, созданный HTML будет иметь следующий вид:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Если атрибут `asp-controller` или `asp-action` не указан, то применяется та же обработка по умолчанию, что и в атрибуте `asp-route`.

### <a name="asp-route"></a>asp-route

`asp-route` позволяет создать URL-адрес, напрямую указывающий на именованный маршрут. С помощью атрибутов маршрутизации маршруту можно присвоить имя, как показано в классе `SpeakerController` и используется в его методе `Evaluations`.

`Name = "speakerevals"` указывает вспомогательной функции тега привязки на необходимость создания маршрута непосредственно к этому методу контроллера с `/Speaker/Evaluations` в URL-адресе. Если наряду с атрибутом `asp-route` указаны атрибут `asp-controller` или `asp-action`, созданный маршрут может отличаться от ожидаемого. Во избежание конфликта маршрута атрибут `asp-route` нельзя использовать вместе с атрибутом `asp-controller` или `asp-action`.

### <a name="asp-all-route-data"></a>asp-all-route-data

Атрибут `asp-all-route-data` позволяет создавать словарь пар "ключ-значение", где ключ является именем параметра, а значение — значением, связанным с этим ключом.

Как показано в следующем примере, создается встроенный словарь, и данные передаются в представление Razor. Кроме того, данные могут быть переданы с помощью модели.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

Приведенный выше код создает следующий URL-адрес: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

При щелчке ссылки вызывается метод контроллера `EvaluationsCurrent`. Это происходит потому, что у этого контроллера есть два строковых параметра, которые соответствуют содержимому, созданному из словаря `asp-all-route-data`.

Если ключи в словаре совпадают с параметрами маршрута, эти значения будут соответствующим образом заменены в маршруте, а в качестве параметров запроса будут созданы другие несовпадающие значения.

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` определяет фрагмент URL-адреса, добавляемый к URL-адресу. Вспомогательная функция тега привязки добавит символ решетки (#). Если создается тег:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

Созданный URL-адрес будет иметь следующий вид: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Хэштеги полезны при создании приложений на стороне клиента. Например, их можно использовать для простоты пометки и поиска на языке JavaScript.

### <a name="asp-area"></a>asp-area

`asp-area` указывает имя области, используемой платформой ASP.NET Core для задания соответствующего маршрута. Ниже приведены примеры, демонстрирующие, как атрибут области приводит к повторному сопоставлению маршрутов. При установке для атрибута `asp-area` значения Blogs перед маршрутами связанных контроллеров и представлений для этого тега привязки добавляется каталог `Areas/Blogs`.

* Имя проекта
  * wwwroot
  * Области
    * Блоги
      * Контроллеры
        * HomeController.cs
      * Представления
        * Главная страница
          * Index.cshtml
          * AboutBlog.cshtml
  * Контроллеры

Указание допустимого тега области, например ```area="Blogs"```, при ссылке на файл ```AboutBlog.cshtml``` будет выглядеть следующим образом при использовании вспомогательной функции тега привязки.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Созданный код HTML будет содержать сегмент областей и иметь следующий вид:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Чтобы области MVC работали в веб-приложении, в шаблон маршрута необходимо включить ссылку на область, если она существует. Этот шаблон, который является вторым параметром вызова метода `routes.MapRoute`, будет иметь следующий вид: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

Атрибут `asp-protocol` предназначен для указания протокола (например, `https`) в URL-адресе. Пример вспомогательной функции тега привязки, содержащей протокол, будет выглядеть следующим образом:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

и создаст следующий код HTML:

```<a href="https://localhost/Home/About">About</a>```

Доменом в примере является localhost, но при формировании URL-адреса вспомогательная функция тега привязки использует общедоступный домен веб-сайта.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Области](xref:mvc/controllers/areas)
