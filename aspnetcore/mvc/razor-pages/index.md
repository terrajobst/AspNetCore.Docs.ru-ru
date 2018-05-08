---
title: Введение в Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 08866543d5b510b86c6af1896a9bd41ae0053ecf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Введение в Razor Pages в ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Райан Новак](https://github.com/rynowak) (Ryan Nowak)

Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.

Если вам нужно руководство, использующее подход "модель-представление-контроллер", см. статью [Начало работы с MVC в ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc).

Этот документ содержит вводные сведения о Razor Pages. Это не пошаговое руководство. Если некоторые разделы покажутся вам слишком сложными, см. [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start). Общие сведения об ASP.NET Core см. в разделе [Введение в ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Создание проекта Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Подробные инструкции по созданию проекта Razor Pages с помощью Visual Studio см. в статье [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Из командной строки выполните команду `dotnet new razor`.

Откройте созданный файл *.csproj* в Visual Studio для Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code) 

Из командной строки выполните команду `dotnet new razor`.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli) 

Из командной строки выполните команду `dotnet new razor`.

---

## <a name="razor-pages"></a>Razor Pages

Функция Razor Pages активируется в файле *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Рассмотрим простую страницу: <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Представленный выше код выглядит как файл представления Razor и отличается от него только директивой `@page`. Директива `@page` превращает файл в действие MVC, а значит обрабатывает запросы напрямую, минуя контроллер. Директива `@page` должна быть первой директивой Razor на странице. Директива `@page` влияет на поведение всех остальных конструкций Razor.

Похожая страница с использованием класса `PageModel` показана в следующих двух файлах. Файл *Pages/Index2.cshtml*:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

Модель страницы *Pages/Index2.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Как правило, файл класса `PageModel` называется так же, как файл Razor Pages, но с расширением *.cs*. Например, представленная выше страница Razor Pages называется *Pages/Index2.cshtml*. Файл, содержащий класс `PageModel`, называется *Pages/Index2.cshtml.cs*.

Сопоставления URL-адресов со страницами определяются расположением конкретной страницы в файловой системе. В приведенной ниже таблице показаны пути Razor Pages и соответствующие URL-адреса.

| Имя файла и путь               | Соответствующий URL |
| ----------------- | ------------ |
| */Pages/index.cshtml* | `/` или `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` или `/Store/Index` |

Примечания.

* Среда выполнения по умолчанию ищет файлы Razor Pages в папке *Pages*.
* Если в URL-адресе не указана конкретная страница, по умолчанию открывается страница `Index`.

## <a name="writing-a-basic-form"></a>Создание простой формы

Функция Razor Pages предназначена для упрощения типовых шаблонов, которые используются в браузерах. [Привязки модели](xref:mvc/models/model-binding), [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и вспомогательные методы HTML *отлично работают* со свойствами, определенными в классе Razor Pages. Рассмотрим страницу с простой формой связи для модели `Contact`.

В представленных в этой статье примерах `DbContext` инициализируется в файле [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Модель данных:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Контекст базы данных:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

Файл представления *Pages/Create.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

Модель страницы *Pages/Create.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Как правило, класс `PageModel` называется `<PageName>Model` и находится в том же пространстве имен, что и страница.

Класс `PageModel` позволяет разделять логику страницы и ее представление. Он определяет обработчики страницы для запросов, отправляемых на страницу, а также данные для ее визуализации. Такое разделение позволяет управлять зависимостями страницы путем их [внедрения](xref:fundamentals/dependency-injection) и выполнять [модульное тестирование](xref:testing/razor-pages-testing) страниц.

Страница содержит *метод обработчика* `OnPostAsync`, который выполняется по запросам `POST` (когда пользователь публикует форму). Методы обработчика можно добавить для любой HTTP-команды. Наиболее распространенные обработчики

* `OnGet` — инициализация необходимого для страницы состояния. Пример обработчика [OnGet](#OnGet).
* `OnPost` — обработка отправленных через форму данных.

Суффикс `Async` не является обязательным, но часто используется для асинхронных функций. Код `OnPostAsync` в приведенном выше примере похож на то, что обычно пишут в контроллере. Этот код типичен для Razor Pages. Большинство примитивов MVC, включая [привязку модели](xref:mvc/models/model-binding), [проверку](xref:mvc/models/validation) и результаты действий, являются общими.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Предыдущий метод `OnPostAsync`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Простая схема `OnPostAsync`

Проверка на наличие ошибок проверки.

*  Если ошибок нет, сохранение данных и перенаправление.
*  Если есть ошибки, отображение страницы с сообщениями проверки. Проверка на стороне клиента выполняется точно так же, как в традиционных приложениях MVC ASP.NET Core. Во многих случаях ошибки проверки выявляются на клиентском компьютере и на сервер не отправляются.

После успешного ввода данных метод обработчика `OnPostAsync` вызывает метод обработчика `RedirectToPage`, чтобы получить экземпляр `RedirectToPageResult`. `RedirectToPage` — это новый результат действия, аналогичный `RedirectToAction` или `RedirectToRoute`, но настроенный для страниц. В приведенном выше примере он выполняет перенаправление на корневую страницу индекса (`/Index`). Более подробно `RedirectToPage` рассматривается в разделе [Создание URL для страниц](#url_gen).

Если в отправленной форме имеются ошибки проверки (переданные на сервер), метод обработчика `OnPostAsync` вызывает метод обработчика `Page`. `Page` возвращает экземпляр `PageResult`. Возвращение `Page` аналогично тому, как действия в контроллерах возвращают `View`. `PageResult` — значение типа возвращаемого значения <!-- Review  --> по умолчанию для метода обработчика. Метод обработчика, вернувший `void`, визуализирует страницу.

Для указания согласия на привязку модели в свойстве `Customer` используется атрибут `[BindProperty]`.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

По умолчанию функция Razor Pages привязывает свойства ко всем командам, кроме GET. Привязка к свойствам позволяет сократить объем необходимого кода. Привязка уменьшает код за счет того, что для визуализации полей формы (`<input asp-for="Customer.Name" />`) и получения входных данных используется одно и то же свойство.

> [!NOTE]
> В целях обеспечения безопасности следует привязать данные запросов GET к свойствам модели страниц. Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами. Эти действия помогут при создании функций, использующих строки запросов и значения маршрутов.
>
> Чтобы привязать свойство к запросам GET, задайте для свойства `SupportsGet` атрибута `[BindProperty]` значение `true`: `[BindProperty(SupportsGet = true)]`

Домашняя страница (*Index.cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Файл кода программной части *Index.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

Файл *Index.cshtml* содержит следующую разметку для создания ссылки на правку для каждого контактного лица.

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) использует атрибут `asp-route-{value}` для создания ссылки на страницу редактирования. Эта ссылка содержит данные о маршруте с идентификатором контактного лица. Например, `http://localhost:5000/Edit/1`.

Файл *Pages/Edit.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

Первая строка содержит директиву `@page "{id:int}"`. Ограничение маршрутизации `"{id:int}"` указывает, что страница должна принимать обращенные к ней запросы, которые содержат данные о маршруте `int`. Если запрос к странице не содержит данные о маршруте, которые можно конвертировать в `int`, среда выполнения возвращает ошибку HTTP 404 (не найдено). Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:

 ```cshtml
@page "{id:int?}"
```

Файл *Pages/Edit.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

Файл *Index.cshtml* также содержит разметку для создания кнопки удаления у каждого контакта клиента:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Во время обработки кнопки удаления в HTML ее `formaction` включает параметры для следующего:

* Идентификатор контакта клиента, указанный атрибутом `asp-route-id`.
* Параметр `handler`, указанный атрибутом `asp-page-handler`.

Пример отображенной кнопки удаления с идентификатором контакта клиента `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

При выборе кнопки на сервер отправляется запрос формы `POST`. По соглашению имя метода обработчика выбирается на основе значения параметра `handler` в соответствии со схемой `OnPost[handler]Async`.

Так как `handler` — `delete` в этом примере, метод обработчика `OnPostDeleteAsync` используется для обработки запроса `POST`. Если `asp-page-handler` имеет другое значение, например `remove`, выбирается метод обработчика страницы с именем `OnPostRemoveAsync`.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

Метод `OnPostDeleteAsync`:

* Принимает `id` из строки запроса.
* Отправляет в базу данных запрос контакта клиента с `FindAsync`.
* Если контакт клиента найден, он удаляется из списка контактов. База данных обновляется.
* Вызывает `RedirectToPage` для перенаправления на корневую страницу индекса (`/Index`).

::: moniker range=">= aspnetcore-2.1"
## <a name="manage-head-requests-with-the-onget-handler"></a>Управление запросами HEAD с помощью обработчика OnGet

Обработчик HEAD обычно создается и вызывается для выполнения запросов HEAD:

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

Если обработчик HEAD (`OnHead`) не определен, Razor Pages выполнит вызов обработчика страниц GET (`OnGet`) в ASP.NET Core 2.1 или более поздней версии. Вы можете явно выбрать такое поведение с помощью [метода SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) в `Startup.Configure` ASP.NET Core 2.1–2.x:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

`SetCompatibilityVersion` задает для параметра `AllowMappingHeadRequestsToGetHandler` Razor Pages значение `true`. Это поведение можно выбрать в версиях ниже ASP.NET Core 3.0 (предварительная версия 1). Каждая основная версия ASP.NET Core наследует поведение выпуска исправлений предыдущей версии.

Вы можете явно не выбирать поведение выпусков исправлений 2.1–2.x, используя конфигурацию приложения, которая сопоставляет запросы HEAD для обработчика GET. Задайте для параметра `AllowMappingHeadRequestsToGetHandler` Razor Pages значение `true`, не вызывая `SetCompatibilityVersion` в `Startup.Configure`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF и Razor Pages

Вам не придется писать отдельный код для [проверки подлинности запросов](xref:security/anti-request-forgery). Razor Pages включает создание и проверку маркеров защиты от подделок по умолчанию.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Использование макетов, частичных реплик, шаблонов и вспомогательных функций тегов с Razor Pages

Pages работает со всеми функциями представлений Razor. Макеты, частичные реплики, шаблоны, вспомогательные функции тегов, а также файлы *_ViewStart.cshtml* и *_ViewImports.cshtml* работают точно так же, как и в стандартных представлениях Razor.

Давайте упростим нашу страницу с помощью некоторых из этих функций.

Добавим [макет страницы](xref:mvc/views/layout) в файл *Pages/_Layout.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

Этот [макет](xref:mvc/views/layout):

* управляет макетом каждой страницы (кроме страниц с отказом от макета);
* импортирует HTML-структуры, такие как JavaScript и таблицы стилей.

Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).

Свойство [Layout](xref:mvc/views/layout#specifying-a-layout) определяется в файле *Pages/_ViewStart.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

**Примечание.** Макет хранится в папке *Pages*. Pages ищет другие представления (макеты, шаблоны, частичные реплики) в иерархическом порядке, начиная с той папки, где находится текущая страница. Макет в папке *Pages* можно использовать на любой странице Razor, которая находится в папке *Pages*.

Корпорация Майкрософт рекомендует **не** размещать файл макета в папке *Views/Shared*. *Views/Shared* — это шаблон представлений MVC. Razor Pages опирается на иерархию папок, а не на условные обозначения путей.

Поиск представлений в Razor Pages охватывает папку *Pages*. Макеты, шаблоны и частичные реплики *работают* с контроллерами MVC и стандартными представлениями Razor.

Добавим файл *Pages/_ViewImports.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` описывается далее в этом руководстве. Директива `@addTagHelper` добавляет [встроенные вспомогательные теги](xref:mvc/views/tag-helpers/builtin-th/Index) на все страницы в папке *Pages*.

<a name="namespace"></a>

Явное применение директивы `@namespace` на странице:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Данная директива задает пространство имен для страницы. Включать пространство имен в директиву `@model` не требуется.

Если директива `@namespace` содержится в файле *_ViewImports.cshtml*, указанное пространство имен определяет префикс для созданного в Pages пространства имен, куда импортируется директива `@namespace`. Остальная часть созданного пространства имен (суффикс) представляет собой разделенный точками относительный путь между папкой с файлом *_ViewImports.cshtml* и папкой, содержащей страницу.

Например, файл кода программной части *Pages/Customers/Edit.cshtml.cs* задает пространство имен явно:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

Файл *Pages/_ViewImports.cshtml* задает следующее пространство имен:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Сформированное пространство имен для файла *Pages/Customers/Edit.cshtml* Razor Pages совпадает с именем файла кода программной части. Директива `@namespace` разработана таким образом, чтобы добавляемые в проект классы C# и сгенерированный страницами код *просто работали* без добавления директивы `@using` для файла кода программной части.

**Примечание.** `@namespace` также работает со стандартными представлениями Razor.

Исходный файл представления *Pages/Create.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Обновленный файл представления *Pages/Create.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Начальный проект Razor Pages](#rpvs17) содержит файл *Pages/_ValidationScriptsPartial.cshtml*, который подключает проверку на стороне клиента.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Формирование URL-адресов для страниц

На представленной выше странице `Create` используется `RedirectToPage`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Это приложение имеет следующую структуру файлов и папок:

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

После успешного выполнения страницы *Pages/Customers/Create.cshtml* и *Pages/Customers/Edit.cshtml* перенаправляются на страницу *Pages/Index.cshtml*. Строка `/Index` составляет часть URL-адреса для доступа к указанной выше странице. Строка `/Index` позволяет создавать URL-адреса для страницы *Pages/Index.cshtml*. Пример:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Имя страницы — это путь к странице из корневой папки */Pages* (включая начальный символ `/`, например `/Index`). Приведенные выше примеры создания URL-адресов задействуют гораздо больше функций, чем просто прописывание URL в коде. Формирование URL-адресов включает [маршрутизацию](xref:mvc/controllers/routing) и позволяет генерировать и включать в код параметры в зависимости от того, как определяется маршрут в пути назначения.

Формирование URL-адресов для страниц поддерживает относительные имена. В приведенной ниже таблице показано, какая страница индекса выбирается с каждым параметром `RedirectToPage` в *Pages/Customers/Create.cshtml*.

| RedirectToPage(x)| Страница |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")` и `RedirectToPage("../Index")` — это <em>относительные имена</em>. Для получения имени целевой страницы параметр `RedirectToPage` <em>комбинируется</em> с путем текущей страницы.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Привязка относительных имен полезна при создании сайтов со сложной структурой. Если для связи между страницами в определенной папке используются относительные имена, эту папку можно переименовать. При этом все ссылки останутся рабочими (так как не включают имя папки).

## <a name="tempdata"></a>TempData

ASP.NET Core позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller). Это свойство хранит данные до тех пор, пока они не будут прочитаны. Для проверки данных без удаления можно использовать методы `Keep` и `Peek`. `TempData` удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.

Атрибут `[TempData]` появился в ASP.NET 2.0 и поддерживается в контроллерах и на страницах.

В следующем коде значение `Message` задается с помощью `TempData`:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Представленная ниже разметка в файле *Pages/Customers/Index.cshtml* отображает значение `Message` с помощью `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

Модель страницы *Pages/Customers/Index.cshtml.cs* применяет атрибут `[TempData]` к свойству `Message`.

```cs
[TempData]
public string Message { get; set; }
```

Дополнительные сведения см. в разделе [TempData](xref:fundamentals/app-state#temp).

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Несколько обработчиков на страницу

Следующая страница формирует разметку для двух обработчиков страницы с помощью вспомогательного тега `asp-page-handler`:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Форма в предыдущем примере включает две кнопки отправки, каждая из которых отправляет данные на отдельный URL-адрес с помощью `FormActionTagHelper`. Атрибут `asp-page-handler` является дополнением к `asp-page`. Атрибут `asp-page-handler` формирует URL-адреса, ,которые используются для отправки данных в каждый из методов обработчиков, определенных страницей. `asp-page` не задается, так как пример сопоставлен с текущей страницей.

Модель страницы

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

В представленном выше коде используются *именованные методы обработчика*. Именованные методы обработчика создаются путем размещения определенного текста в имени после `On<HTTP Verb>` и перед `Async` (если есть). В приведенном выше примере использовались методы страницы OnPost**JoinList**Async и OnPost**JoinListUC**Async. Если убрать *OnPost* и *Async*, имена обработчиков будут выглядеть как `JoinList` и `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

При использовании представленного выше кода URL-путь для отправки данных в `OnPostJoinListAsync` будет выглядеть как `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. URL-путь для отправки данных в `OnPostJoinListUCAsync` будет иметь вид `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.



## <a name="customizing-routing"></a>Настройка маршрутизации

Если вы не хотите, чтобы в URL-адресе отображалась строка запроса `?handler=JoinList`, измените маршрут таким образом, чтобы в качестве пути в URL-адресе указывалось имя обработчика. Для настройки маршрута можно добавить после директивы `@page` шаблон маршрута, заключенные в двойные кавычки.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

В представленном выше маршруте вместо строки запроса URL-путь содержит имя обработчика. Символ `?` после `handler` означает, что параметр маршрута является необязательным.

С помощью директивы `@page` можно добавить к маршруту страницы дополнительные сегменты и параметры. Все, что вы добавите, будет **присоединено** к маршруту страницы по умолчанию. Использовать для изменения маршрута страницы абсолютный или виртуальный путь (например, вида `"~/Some/Other/Path"`) нельзя.

## <a name="configuration-and-settings"></a>Конфигурация и параметры

Чтобы настроить дополнительные параметры, воспользуйтесь методом расширения `AddRazorPagesOptions` в построителе MVC:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

В настоящее время `RazorPagesOptions` позволяет задавать корневую папку для страниц и добавлять для них обозначения моделей приложений. В дальнейшем мы расширим спектр его возможностей.

Сведения о предварительной компиляции представлений см. в статье [Компиляция представлений Razor](xref:mvc/views/view-compilation).

[Загрузить или просмотреть пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).

См. раздел [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), который дает дополнительную информацию к введению.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Указание местонахождения Razor Pages в корне каталога

По умолчанию Razor Pages находится в корне каталога */Pages*. Добавьте [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в корне содержимого Razor Pages ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) приложения:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Указание местонахождения Razor Pages в пользовательском корневом каталоге

Добавьте [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_), чтобы указать, что Razor Pages находится в пользовательском корневом каталоге в приложении (укажите относительный путь):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a>См. также

* [Введение в ASP.NET Core](xref:index)
* [Синтаксис Razor](xref:mvc/views/razor)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Соглашения об авторизации Razor Pages](xref:security/authorization/razor-pages-authorization)
* [Пользовательские поставщики моделей маршрутов и страниц Razor Pages](xref:mvc/razor-pages/razor-pages-convention-features)
* [Модульные и интеграционные тесты для Razor Pages](xref:testing/razor-pages-testing)
