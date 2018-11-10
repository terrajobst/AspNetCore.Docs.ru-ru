---
title: Миграция с ASP.NET MVC для ASP.NET Core MVC
author: ardalis
description: Узнайте, как приступить к миграции проекта ASP.NET MVC в ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 7c9d927bbd06f96f130d53e946a2963b5804960b
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505743"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Миграция с ASP.NET MVC для ASP.NET Core MVC

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Дэниэл рот](https://github.com/danroth27), [Стив Смит](https://ardalis.com/), и [Scott Addie](https://scottaddie.com)

В этой статье показано, как приступить к миграции проекта ASP.NET MVC для [ASP.NET Core MVC](../mvc/overview.md). В процессе он выделяет многие действия, которые были изменены с ASP.NET MVC. Миграция с ASP.NET MVC состоит из нескольких этапов и в этой статье рассматриваются начальной настройки, основные контроллеры и представления, статическое содержимое и зависимости на стороне клиента. Дополнительные статьи посвящены перенос конфигурации, а также найти во многих проектах ASP.NET MVC код удостоверений.

> [!NOTE]
> Номера версий в примерах может потерять свою актуальность. Может потребоваться соответствующим образом обновление проектов.

## <a name="create-the-starter-aspnet-mvc-project"></a>Создание начального проекта ASP.NET MVC

Чтобы продемонстрировать обновления, мы начнем с создания приложения ASP.NET MVC. Создайте его с именем *WebApp1* так, пространство имен проекта ASP.NET Core, мы создадим в следующем шаге.

![Диалоговое окно Visual Studio новый проект](mvc/_static/new-project.png)

![Диалоговое окно создания веб-приложение: шаблон проекта MVC, выбранного в панели шаблонов ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Необязательно:* изменить имя решения из *WebApp1* для *Mvc5*. Visual Studio отображает имя нового решения (*Mvc5*), что упрощает определить этот проект из следующего проекта.

## <a name="create-the-aspnet-core-project"></a>Создание проекта ASP.NET Core

Создайте новый *пустой* веб-приложения ASP.NET Core с тем же именем, что и предыдущий проект (*WebApp1*) чтобы совпадали пространства имен в двух проектах. Наличие то же пространство имен упрощает проще скопировать код между двумя проектами. Вам придется создать этот проект в каталоге, отличном от предыдущий проект, чтобы использовать то же имя.

![Диалоговое окно создания нового проекта](mvc/_static/new_core.png)

![Диалоговое окно создания веб-приложения ASP.NET: шаблон пустого проекта, выбранного в панели шаблоны ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Необязательно:* создать новое приложение ASP.NET Core с помощью *веб-приложение* шаблона проекта. Назовите проект *WebApp1*и выберите параметр проверки из **учетные записи отдельных пользователей**. Переименовать это приложение для *FullAspNetCore*. Создание этого проекта экономит время при преобразовании. Вы можете изучить сгенерированный шаблона код см. в результате или скопировать код для преобразования проекта. Это также полезно, когда вы перестает отвечать на шаг преобразования, сравниваемый с этим проектом, созданных в шаблоне.

## <a name="configure-the-site-to-use-mvc"></a>Настройка сайта для использования MVC

::: moniker range=">= aspnetcore-2.1"

* При нацеливании на .NET Core, [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) указывается по умолчанию. Этот пакет содержит пакеты, часто используемые пакеты по MVC-приложения. Для работы с .NET Framework, ссылки на пакеты должны быть перечислены по отдельности в файле проекта.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* При нацеливании на .NET Core, [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) указывается по умолчанию. Этот пакет содержит пакеты, часто используемые пакеты по MVC-приложения. Для работы с .NET Framework, ссылки на пакеты должны быть перечислены по отдельности в файле проекта.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* При нацеливании на .NET Core или .NET Framework, пакетов, часто используемых пакетов по MVC-приложения, перечислены по отдельности в файле проекта.

::: moniker-end

`Microsoft.AspNetCore.Mvc` — Это платформа ASP.NET Core MVC. `Microsoft.AspNetCore.StaticFiles` — Это обработчик статических файлов. Среда выполнения ASP.NET Core является модульной средой, и вы должны явно согласиться на обслуживать статические файлы (см. в разделе [статические файлы](xref:fundamentals/static-files)).

* Откройте *Startup.cs* и измените код следующим:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles` Метод расширения добавляет обработчик статических файлов. Как упоминалось ранее, среда выполнения ASP.NET является модульной средой, и вы должны явно согласиться на обслуживать статические файлы. `UseMvc` Добавляет метод расширения маршрутизации. Дополнительные сведения см. в разделе [запуск приложения](xref:fundamentals/startup) и [маршрутизации](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Добавьте контроллер и представление

В этом разделе вы добавите это минимальный контроллер и представление для использования в качестве заполнителей для контроллера ASP.NET MVC и представлений, что необходимо перенести в следующем разделе.

* Добавить *контроллеров* папки.

* Добавить **класс контроллера** с именем *HomeController.cs* для *контроллеров* папки.

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/add_mvc_ctl.png)

* Добавить *представления* папки.

* Добавить *Views/Home* папки.

* Добавить **представления Razor** с именем *Index.cshtml* для *Views/Home* папки.

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/view.png)

Ниже показана структура проекта:

![В обозревателе решений файлов и папок из WebApp1](mvc/_static/project-structure-controller-view.png)

Замените содержимое файла *Views/Home/Index.cshtml* файл со следующими:

```html
<h1>Hello world!</h1>
```

Запустите приложение.

![Веб-приложения в Microsoft Edge](mvc/_static/hello-world.png)

См. в разделе [контроллеров](xref:mvc/controllers/actions) и [представления](xref:mvc/views/overview) Дополнительные сведения.

Теперь, когда у нас есть минимальный рабочий проект ASP.NET Core, можно начать перенос функциональные возможности из проекта ASP.NET MVC. Нам нужно переместить следующие:

* клиентским содержимым ("CSS", "Шрифты" и "скрипты")

* контроллеры

* представления

* модели

* Объединение

* фильтры

* Журнал ввода-вывода, Identity (это делается в следующем руководстве.)

## <a name="controllers-and-views"></a>Контроллеры и представления

* Копирование каждого из методов из ASP.NET MVC `HomeController` к новому `HomeController`. Обратите внимание, что в ASP.NET MVC, тип возвращаемого значения метода действия контроллера встроенного шаблона [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); в ASP.NET Core MVC, возвращаемое методы действий `IActionResult` вместо этого. `ActionResult` реализует `IActionResult`, поэтому нет необходимости, чтобы изменить тип возвращаемого значения своих методов действий.

* Копировать *About.cshtml*, *Contact.cshtml*, и *Index.cshtml* файлы представления Razor из проекта ASP.NET MVC в проект ASP.NET Core.

* Запустите приложение ASP.NET Core и каждого метода теста. Мы еще не перенести файл макета или стили еще, поэтому просмотра представления только с содержимым в файлах представления. Будут ссылки файла, созданного макета для `About` и `Contact` просматривает, поэтому вы должны вызывать их из браузера (Замените **4492** с номером порта, используемый в проекте).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Страница](mvc/_static/contact-page.png)

Обратите внимание, отсутствие элементов меню и использования шаблонов. Мы исправим это в следующем разделе.

## <a name="static-content"></a>Статическое содержимое

В предыдущих версиях ASP.NET MVC статическое содержимое было размещено в корне веб-проекта и был смешиваться с файлами на сервере. В ASP.NET Core, статическое содержимое размещается в *wwwroot* папки. Нужно будет скопировать статическое содержимое из старого приложения ASP.NET MVC для *wwwroot* в проекте ASP.NET Core. В этот пример преобразования:

* Копировать *favicon.ico* файл из старого проекта MVC *wwwroot* папки в проекте ASP.NET Core.

ASP.NET MVC старый проект использует [Bootstrap](https://getbootstrap.com/) для его стилей и сохраняет файлы начальной загрузки *содержимого* и *сценариев* папки. Шаблон, который создан старого проекта ASP.NET MVC, ссылается на начальной загрузки в файле макета (*Views/Shared/_Layout.cshtml*). Можно скопировать *bootstrap.js* и *bootstrap.css* проект, файлы из ASP.NET MVC *wwwroot* папку в новом проекте. Вместо этого мы добавим поддержку для начальной загрузки (и другие клиентские библиотеки), с помощью CDN в следующем разделе.

## <a name="migrate-the-layout-file"></a>Перенесите файл макета

* Копировать *_ViewStart.cshtml* файла из старого проекта ASP.NET MVC *представления* папки в проекте ASP.NET Core *представления* папки. *_ViewStart.cshtml* файл не был изменен в ASP.NET Core MVC.

* Создание *Views/Shared* папки.

* *Необязательно:* копирования *_ViewImports.cshtml* из *FullAspNetCore* проект MVC *представления* папки в проекте ASP.NET Core  *Представления* папки. Удалите все объявления пространства имен в *_ViewImports.cshtml* файла. *_ViewImports.cshtml* файл предоставляют пространства имен для всех файлов представлений и переводит [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro). Вспомогательные функции тегов используются в файл макета. *_ViewImports.cshtml* файл появился в ASP.NET Core.

* Копировать *_Layout.cshtml* файла из старого проекта ASP.NET MVC *Views/Shared* папки в проекте ASP.NET Core *Views/Shared* папки.

Откройте *_Layout.cshtml* файл и внесите следующие изменения (ниже приведен полный код):

* Замените `@Styles.Render("~/Content/css")` с `<link>` элемент загрузить *bootstrap.css* (см. ниже).

* Удалите `@Scripts.Render("~/bundles/modernizr")`.

* Закомментируйте `@Html.Partial("_LoginPartial")` строки (заключить строку с `@*...*@`). Дополнительные сведения см. в разделе [перенести проверки подлинности и удостоверения в ASP.NET Core](xref:migration/identity)

* Замените `@Scripts.Render("~/bundles/jquery")` с `<script>` элемент (см. ниже).

* Замените `@Scripts.Render("~/bundles/bootstrap")` с `<script>` элемент (см. ниже).

Разметка замены для включения Bootstrap CSS:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Разметка замены для jQuery и включения JavaScript начальной загрузки:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Обновленный *_Layout.cshtml* файла приведен ниже:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Откройте сайт в браузере. Он должен загружаться корректно, ожидаемый стили на месте.

* *Необязательно:* может потребоваться попробуйте использовать файл макета. Для этого проекта можно скопировать файл макета из *FullAspNetCore* проекта. Использует файл макета [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и другие усовершенствования.

## <a name="configure-bundling-and-minification"></a>Настроить объединение и Минификация

Сведения о способах настройки объединения и минификации, см. в разделе [объединение и Минификация](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Устранить ошибки HTTP 500

Существует множество проблем, которые могут вызвать сообщение об ошибке HTTP 500, которые не содержат сведений о источник проблемы. Например если *Views/_ViewImports.cshtml* файл содержит пространство имен, который не существует в проекте, то вы получите ошибку HTTP 500. По умолчанию в приложениях ASP.NET Core `UseDeveloperExceptionPage` расширение добавляется к `IApplicationBuilder` и выполняется, когда конфигурация *разработки*. Это подробно описано в следующем коде:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core преобразует необработанные исключения в веб-приложения в сообщения об ошибках HTTP 500. Как правило сведения об ошибке не включены в эти ответы для предотвращения раскрытия потенциально конфиденциальной информации о сервере. См. в разделе **с помощью страница исключения для разработчика** в [обработки ошибок](../fundamentals/error-handling.md) Дополнительные сведения.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Клиентская разработка](xref:client-side/index)
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
