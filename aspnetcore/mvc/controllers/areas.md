---
title: Области в ASP.NET Core
author: rick-anderson
description: Сведения о том, что области — это возможность ASP.NET MVC, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833531"
---
# <a name="areas-in-aspnet-core"></a>Области в ASP.NET Core

Авторы: [Дхананджай Кумар](https://twitter.com/debug_mode) (Dhananjay Kumar) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Области — это возможность ASP.NET, которая служит для объединения связанных функций в группу в виде отдельного пространства имен (для маршрутизации) и структуры папок (для представлений). При использовании областей создается иерархия в целях маршрутизации. Для этого к `controller` и `action` или `page` страницы Razor добавляется еще один параметр маршрута, `area`.

Области позволяют разделять веб-приложение ASP.NET Core на более мелкие функциональные группы, каждая из которых имеет свой собственный набор Razor Pages, контроллеров, представлений и моделей. По сути, область является структурой внутри приложения. В веб-проекте ASP.NET Core логические компоненты, например страницы, модель, контроллер и представление, хранятся в разных папках. Среда выполнения ASP.NET Core использует соглашения об именовании для создания связи между этими компонентами. Крупное приложение может быть целесообразно разделить на отдельные высокоуровневые области функциональности. Например, в приложении для электронной коммерции можно выделить несколько подразделений: для оформления заказов, выставления счетов или поиска. Каждое из них имеет собственную область для размещения представлений, контроллеров, Razor Pages и моделей.

Использовать области в проекте рекомендуется в указанных ниже случаях.

* Приложение состоит из нескольких высокоуровневых функциональных частей, которые могут быть разделены логически.
* Необходимо разделить приложение так, чтобы с каждой функциональной областью можно было работать отдельно.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([описание скачивания](xref:index#how-to-download-a-sample)). Пример загрузки содержит простое приложение для тестирования областей.

## <a name="areas-for-controllers-with-views"></a>Области для контроллеров с представлениями

Типичное веб-приложение ASP.NET Core, использующее области, контроллеры и представления, содержит следующие элементы.

* [Структура папки области](#area-folder-structure).
* Контроллеры с атрибутом [&lbrack;Область&rbrack;](#attribute) для привязки контроллера к области: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* [Маршрут к области, добавленный к запуску](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

## <a name="area-folder-structure"></a>Структура папки области
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

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
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

### <a name="link-generation-with-areas"></a>Создание ссылки с областями

В следующем коде из [примера загрузки](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) показано создание ссылок с указанной областью:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Ссылки, создаваемые предыдущим кодом, действуют в любом месте приложения.

Пример загрузки включает [частичное представление](xref:mvc/views/partial), которое содержит указанные выше ссылки и те же ссылки без указания области. Частичное представление указывается в [файле макета](), поэтому каждая страница в приложении отображает созданные ссылки. Ссылки, создаваемые без указания области, допустимы только при обращении со страницы в одной области и контроллере.

Если область или контроллер не указаны, маршрутизация зависит от значений *окружения*. Текущие значения маршрута текущего запроса считаются значениями окружения для создания ссылки. Во многих случаях для примера приложения при использовании значений окружения создаются неверные ссылки.

Дополнительные сведения см. в разделе [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Общий макет для областей с использованием файла _ViewStart.cshtml

Чтобы совместно использовать общий макет для всего приложения, переместите *_ViewStart.cshtml* в корневую папку приложения.

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Измените папку области по умолчанию, где хранятся представления

Следующий код изменяет папку области по умолчанию с `"Areas"` на `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a>Публикация областей

Все файлы `*.cshtml` и `wwwroot/**` публикуются в каталоге выходных данных при включении элемента `<Project Sdk="Microsoft.NET.Sdk.Web">` в файл *CSPROJ*.
