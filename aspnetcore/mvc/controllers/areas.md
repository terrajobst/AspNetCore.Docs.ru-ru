---
title: Области в ASP.NET Core
author: rick-anderson
description: Сведения о том, что области — это возможность ASP.NET MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).
ms.author: riande
ms.date: 08/16/2019
uid: mvc/controllers/areas
ms.openlocfilehash: d0af3092776ee09469c879fffd3047c50b1a59b4
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545805"
---
# <a name="areas-in-aspnet-core"></a>Области в ASP.NET Core

Авторы: [Дхананджай Кумар](https://twitter.com/debug_mode) (Dhananjay Kumar) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Области — это возможность ASP.NET, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений). При использовании областей создается иерархия в целях маршрутизации. Для этого к `controller` и `action` или `page` страницы Razor добавляется еще один параметр маршрута, `area`.

Области позволяют разделять веб-приложение ASP.NET Core на более мелкие функциональные группы, каждая из которых имеет свой собственный набор Razor Pages, контроллеров, представлений и моделей. По сути, область является структурой внутри приложения. В веб-проекте ASP.NET Core логические компоненты, например страницы, модель, контроллер и представление, хранятся в разных папках. Среда выполнения ASP.NET Core использует соглашения об именовании для создания связи между этими компонентами. Крупное приложение может быть целесообразно разделить на отдельные высокоуровневые области функциональности. Например, в приложении для электронной коммерции можно выделить несколько подразделений: для оформления заказов, выставления счетов или поиска. Каждое из них имеет собственную область для размещения представлений, контроллеров, Razor Pages и моделей.

Использовать области в проекте рекомендуется в таких случаях:

* Приложение состоит из нескольких высокоуровневых функциональных частей, которые могут быть разделены логически.
* Необходимо разделить приложение так, чтобы с каждой функциональной областью можно было работать отдельно.

[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([описание скачивания](xref:index#how-to-download-a-sample)). Пример загрузки содержит простое приложение для тестирования областей.

Если вы используете Razor Pages, см. раздел [Области c Razor Pages](#areas-with-razor-pages) далее в этом документе.

## <a name="areas-for-controllers-with-views"></a>Области для контроллеров с представлениями

Типичное веб-приложение ASP.NET Core, использующее области, контроллеры и представления, содержит следующие элементы.

* [Структура папки области](#area-folder-structure).
* Контроллеры с атрибутом [&lbrack;Область&rbrack;](#attribute) для привязки контроллера к области:

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* [Маршрут к области, добавленный к запуску](#add-area-route):

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a>Структура папки области

Рассмотрим приложение с двумя логическими группами: *товары* и *услуги*. При использовании областей структура папок будет выглядеть следующим образом.

* Имя проекта
  * Области
    * Продукты
      * Контроллеры
        * HomeController.cs
        * ManageController.cs
      * Представления
        * Главная страница
          * Index.cshtml
        * Управление
          * Index.cshtml
          * About.cshtml
    * Службы
      * Контроллеры
        * HomeController.cs
      * Представления
        * Главная страница
          * Index.cshtml

Хотя предыдущий макет часто используется при работе с областями, только файлы представления необходимы для использования этой структуры папок. Обнаружение подходящего файла представления области происходит в следующем порядке.

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

Расположение папок не для представления, например для *контроллеров* и *моделей*, **не** рассматривается. Например, папка *Контроллеры* и *Модели* не требуется. В папках *Контроллеры* и *Модели* содержится код, который компилируется в DLL-файл. Содержимое папки *Представления* не компилируется, пока не будет сделан запрос к этому представлению.

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>Привязка контроллера к области

Контроллеры областей назначаются с помощью атрибута [&lbrack;Область&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>Добавление маршрута области

Маршруты области обычно используют маршрутизацию на основе соглашений, а не атрибутов. При маршрутизации на основе соглашений учитывается порядок. Как правило, маршруты с областями должны находиться раньше в таблице маршрутов, так как они более точные, чем маршруты без областей.

`{area:...}` можно использовать как токен в шаблонах маршрутов, если пространство URL-адресов одинаково во всех областях:

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

В приведенном выше коде `exists` применяет ограничение, связанное с тем, что маршрут должен соответствовать области. Использование `{area:...}` — это наименее сложный механизм для добавления маршрута к областям.

В следующем коде используется <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> для создания двух именованных маршрутов областей:

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

При использовании `MapAreaRoute` с ASP.NET Core 2.2 см. [эту задачу GitHub](https://github.com/aspnet/AspNetCore/issues/7772).

Дополнительные сведения см. в разделе [Маршрутизация области](xref:mvc/controllers/routing#areas).

### <a name="link-generation-with-mvc-areas"></a>Создание ссылки с областями MVC

В следующем коде из [примера загрузки](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) показано создание ссылок с указанной областью:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Ссылки, создаваемые предыдущим кодом, действуют в любом месте приложения.

Пример загрузки включает [частичное представление](xref:mvc/views/partial), которое содержит указанные выше ссылки и те же ссылки без указания области. Частичное представление указывается в [файле макета](xref:mvc/views/layout), поэтому каждая страница в приложении отображает созданные ссылки. Ссылки, создаваемые без указания области, допустимы только при обращении со страницы в одной области и контроллере.

Если область или контроллер не указаны, маршрутизация зависит от значений *окружения*. Текущие значения маршрута текущего запроса считаются значениями окружения для создания ссылки. Во многих случаях для примера приложения при использовании значений окружения создаются неверные ссылки.

Дополнительные сведения см. в разделе [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a>Общий макет для областей с использованием файла _ViewStart.cshtml

Чтобы совместно использовать общий макет для всего приложения, переместите *_ViewStart.cshtml* в корневую папку приложения.

### <a name="_viewimportscshtml"></a>_ViewImports.cshtml

В стандартном расположении файл */Views/_ViewImports.cshtml* не применяется к областям. Чтобы использовать общие объекты [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using` или `@inject` в области, необходимо, чтобы соответствующий файл *_ViewImports.cshtml* [применялся к представлениям области](xref:mvc/views/layout#importing-shared-directives). Чтобы поведение во всех представлениях было одинаковым, переместите файл */Views/_ViewImports.cshtml* в корневую папку приложения.

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Измените папку области по умолчанию, где хранятся представления

Следующий код изменяет папку области по умолчанию с `"Areas"` на `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a>Области c Razor Pages

Для создания областей с Razor Pages необходима папка *Areas/<area name>/Pages* в корневом каталоге приложения. В [этом примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) используется следующая структура папок:

* Имя проекта
  * Области
    * Продукты
      * Pages
        * _ViewImports
        * Общие сведения о
        * Индекс
    * Службы
      * Pages
        * Управление
          * Общие сведения о
          * Индекс

### <a name="link-generation-with-razor-pages-and-areas"></a>Создание ссылки с Razor Pages и областями

В следующем коде из [этого примера](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) показано создание ссылок с указанной областью (например, `asp-area="Products"`).

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

Ссылки, создаваемые предыдущим кодом, действуют в любом месте приложения.

Пример загрузки включает [частичное представление](xref:mvc/views/partial), которое содержит указанные выше ссылки и те же ссылки без указания области. Частичное представление указывается в [файле макета](xref:mvc/views/layout), поэтому каждая страница в приложении отображает созданные ссылки. Ссылки, создаваемые без указания области, допустимы только при обращении со страницы в одной области.

Если область не указана, маршрутизация зависит от значений *окружения*. Текущие значения маршрута текущего запроса считаются значениями окружения для создания ссылки. Во многих случаях для примера приложения при использовании значений окружения создаются неверные ссылки. Для примера рассмотрим ссылки, созданные с помощью следующего кода.

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

В приведенном выше коде:

* Ссылка, созданная с помощью `<a asp-page="/Manage/About">` будет правильной, только если последний запрос был к странице в области `Services`. Например `/Services/Manage/`, `/Services/Manage/Index` или `/Services/Manage/About`.
* Ссылка, созданная с помощью `<a asp-page="/About">` будет правильной, только если последний запрос был к странице в области `/Home`.
* Это код из [этого примера](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a>Импорт пространства имен и вспомогательных функций тегов с помощью файла _ViewImports

Файл *_ViewImports.cshtml* файл можно добавить к каждой области папки *Pages* для импорта пространства имен и вспомогательных функций тегов в каждую папку страницы Razor Pages.

Рассмотрите область *служб* в примере кода, которая не содержит файл *_ViewImports.cshtml*. В следующем примере показана разметка страницы с кодом Razor */Services/Manage/About*.

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

В этой разметке:

* Для указания модели (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`) необходимо использовать полное доменное имя.
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) включены с помощью `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`.

В примере загрузка область продуктов содержит следующий файл *_ViewImports.cshtml*:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

В следующем примере показана разметка страницы с кодом Razor */Products/About*:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

В предыдущем файле пространство имен и директива `@addTagHelper` импортируются в файл с помощью файла *Areas/Products/Pages/_ViewImports.cshtml*.

Дополнительные сведения см. в разделах [Управление областью действия вспомогательной функции тега](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) и [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives).

### <a name="shared-layout-for-razor-pages-areas"></a>Общий макет для областей Razor Pages

Чтобы совместно использовать общий макет для всего приложения, переместите *_ViewStart.cshtml* в корневую папку приложения.

### <a name="publishing-areas"></a>Публикация областей

Все файлы CSHTML и файлы в каталоге *wwwroot* публикуются в выходных данных, если в файл CSPROJ включен `<Project Sdk="Microsoft.NET.Sdk.Web">`.
