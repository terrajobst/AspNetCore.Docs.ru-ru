---
title: Учебник. Вызов веб-API ASP.NET Core с помощью jQuery
author: rick-anderson
description: Узнайте, как вызывать веб-API ASP.NET Core, используя jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602573"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>Учебник. Вызов веб-API ASP.NET Core с помощью jQuery

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этот учебник описывает вызов веб-API ASP.NET Core с использованием jQuery.

::: moniker range="< aspnetcore-3.0"

При использовании ASP.NET Core 2.2 см. учебник [Вызов веб-API с помощью jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery) для версии 2.2.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Предварительные требования

Изучите [Учебник. Создание веб-API](xref:tutorials/first-web-api).

## <a name="call-the-api-with-jquery"></a>Вызов API с помощью jQuery

В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API. jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.

Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

Создайте папку *wwwroot* в каталоге проекта.

Добавьте HTML-файл *index.html* в каталог *wwwroot*. Замените его содержимое следующей разметкой:

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*. Замените его содержимое следующим кодом:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:

* Откройте файл *Properties\launchSettings.json*.
* Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.

Для получения jQuery можно использовать следующие способы. В предыдущем фрагменте кода библиотека загружается из CDN.

В этом примере вызываются все методы CRUD в API. Ниже приводится пояснение вызовов API.

### <a name="get-a-list-of-to-do-items"></a>Получение списка элементов задач

Функция jQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `GET` к API, который возвращает JSON, представляющий массив элементов списка дел. В случае успешного запроса используется функция обратного вызова `success`. При обратном вызове в модель DOM вносятся данные о задачах.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Добавление элемента задачи

Функция [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `POST` с элементом списка дел в теле запроса. Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке. Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Обновление элемента задачи

Обновление элемента списка дел выполняется аналогично его добавлению. `url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Удаление элемента задачи

Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

Перейдите к следующему учебнику, посвященному созданию страниц справки по API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end