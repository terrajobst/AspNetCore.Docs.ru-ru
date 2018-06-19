---
title: Вспомогательная функция тега частичного представления в ASP.NET Core
author: scottaddie
description: Сведения о вспомогательной функции тега частичного представления в ASP.NET и роли каждого из его атрибутов в отрисовке частичного представления.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 786c333980db89a9a5a60dc70c0bd1998ca159cd
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962598"
---
# <a name="partial-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега частичного представления в ASP.NET Core

Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Обзор

Вспомогательная функция тега частичного представления используется для отрисовки [частичного представления](xref:mvc/views/partial) в приложениях Razor Pages и MVC. Примечания.

* Требуется ASP.NET Core 2.1 или более поздней версии.
* Это альтернатива [синтаксиса вспомогательной функции HTML](xref:mvc/views/partial#referencing-a-partial-view).
* Отрисовка частичного представления выполняется асинхронно.

Параметры вспомогательной функции HTML для отрисовки частичного представления:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

В примерах в этом документе используется модель *Product*:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Далее следуют атрибуты вспомогательной функции тега частичного представления.

## <a name="name"></a>имя

Атрибут `name` является обязательным. Она указывает имя или путь для отображения частичного представления, которое будет отрисовано. Если указано имя частичного представления, запускается процесс [обнаружения представления](xref:mvc/views/overview#view-discovery). Этот процесс пропускается, если предоставлен явный путь.

В приведенной ниже разметке используется явный путь, указывающий, что файл *_ProductPartial.cshtml* необходимо загрузить из папки *Shared*. При использовании атрибута [for](#for) модель передается в частичное представление для привязки.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a>for

Атрибут `for` назначает [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) для сравнения с текущей моделью. Класс `ModelExpression` выводит синтаксис `@Model.`. Например, можно использовать `for="Product"` вместо `for="@Model.Product"`. Это поведение вывода по умолчанию переопределяется с помощью символа `@` для определения встроенного выражения. Атрибут `for` нельзя использовать с атрибутом [model](#model).

Приведенная ниже разметка загружает *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

Частичное представление привязывается к свойству `Product` соответствующей модели страницы:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a>model

Атрибут `model` присваивает экземпляр модели для передачи в частичное представление. Атрибут `model` нельзя использовать с атрибутом [for](#for).

В следующей разметке создается экземпляр нового объекта `Product`, который передается атрибуту `model` для привязки:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a>view-data

Атрибут `view-data` присваивает [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) для передачи в частичное представление. В следующей разметке вся коллекция ViewData становится доступной для частичного представления:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

В приведенном выше коде для значения ключа `IsNumberReadOnly` устанавливается `true`, и значение добавляется в коллекцию ViewData. Следовательно, `ViewData["IsNumberReadOnly"]` становится доступно в следующем частичном представлении:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

В этом примере значение `ViewData["IsNumberReadOnly"]` определяет, будет ли поле *Number* отображаться только для просмотра.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Частичные представления](xref:mvc/views/partial)
* [Нестрого типизированные данные (ViewData, атрибут ViewData и ViewBag)](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
