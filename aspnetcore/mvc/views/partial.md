---
title: Частичные представления в ASP.NET Core
author: ardalis
description: Сведения о том, что частичное представление — это представление, отображаемое в другом представлении, и о сценариях их использования в приложениях ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 9f90ce39929d0dbc216b47d76d652c1fca866ec2
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889120"
---
# <a name="partial-views-in-aspnet-core"></a>Частичные представления в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith), [Махер Джендуби](https://twitter.com/maherjend) (Maher JENDOUBI), [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Скотт Саубер](https://twitter.com/scottsauber) (Scott Sauber)

В ASP.NET Core MVC поддерживаются частичные представления, которые удобны для совместного использования повторяющихся частей на веб-страницах в разных представлениях.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Что такое частичные представления

Частичное представление — это представление, которое преобразовывается для просмотра внутри другого представления. Выходные данные HTML, создаваемые при выполнении частичного представления, передаются в вызывающее (родительское) представление. Как и для обычных представлений, для частичных представлений используется расширение имени файла *CSHTML*.

Например, шаблон проекта **веб-приложения** ASP.NET Core 2.1 содержит частичное представление *_CookieConsentPartial.cshtml*. Частичное представление загружается из *_Layout.cshtml*.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Когда следует использовать частичные представления

Частичные представления — это эффективный способ разбиения больших представлений на более простые компоненты. Они позволяют сократить дублирование содержимого и использовать элементы представлений повторно. Общие элементы макета следует указывать в файле [_Layout.cshtml](xref:mvc/views/layout). Повторно используемое содержимое, не относящееся к макету, можно инкапсулировать в частичных представлениях.

На сложной странице, состоящей из нескольких логических частей, удобнее работать с каждой из них как с отдельным частичным представлением. Каждый фрагмент страницы можно просмотреть отдельно от остальной страницы. Представление самой страницы становится проще, так как оно содержит только общую структуру страницы и вызовы для отображения частичных представлений.

> [!TIP]
> Соблюдайте в представлениях [принцип "не повторяйся"](https://deviq.com/don-t-repeat-yourself/).

## <a name="declare-partial-views"></a>Объявление частичных представлений

Частичные представления создаются так же, как и обычные: &mdash;нужно создать файл *CSHTML* в папке *Views*. Смыслового различия между частичным и обычным представлениями нет — они просто по-разному преобразовываются для просмотра. Например, представление, возвращаемое из объекта [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), может применяться как частичное представление. Основное различие между преобразованием обычных и частичных представлений для просмотра заключается в том, что частичные представления не запускают файл *_ViewStart.cshtml*. Обычные представления запускают файл *_ViewStart.cshtml*. Дополнительные сведения о *_ViewStart.cshtml* см. в разделе [Макет](xref:mvc/views/layout)).

Как правило, имена файлов частичного представления начинаются с `_`. Это соглашение об именовании не является обязательным требованием, но помогает отличать частичные представления от обычных.

## <a name="reference-a-partial-view"></a>Ссылка на частичное представление

На странице представления частичное представление может преобразовываться для просмотра несколькими способами. Мы рекомендуем использовать асинхронную подготовку к просмотру.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Вспомогательная функция тега частичного представления

Для вспомогательной функции тега частичного представления требуется ASP.NET Core 2.1 или более поздней версии. Она преобразует страницы для просмотра асинхронно и использует синтаксис типа HTML.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Дополнительные сведения см. в разделе <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Асинхронный вспомогательный метод HTML

Если вы используете вспомогательный метод HTML, мы рекомендуем использовать [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Он возвращает тип [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) в оболочке `Task`. Метод указывается с помощью префикса вызова `@`.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

Вы также можете выполнить преобразование для просмотра частичного представления с помощью [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Этот метод не возвращает результат. Он передает выводимые данные непосредственно в ответ в потоковом режиме. Так как этот метод не возвращает результат, его необходимо вызывать в блоке кода Razor.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Так как метод `RenderPartialAsync` передает результат в потоковом режиме напрямую, в некоторых ситуациях он может быть эффективнее. Тем не менее рекомендуется использовать `PartialAsync`.

### <a name="synchronous-html-helper"></a>Синхронный вспомогательный метод HTML

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) и [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) — это синхронные эквиваленты `PartialAsync` и `RenderPartialAsync` соответственно. Не рекомендуется использовать синхронные эквиваленты, поскольку в некоторых случаях возникает взаимоблокировка. Будущие выпуски не будут содержать синхронные методы.

> [!IMPORTANT]
> Если представления должны выполнять код, используйте [компонент представления](xref:mvc/views/view-components) вместо частичного представления.

::: moniker range=">= aspnetcore-2.1"

В ASP.NET Core 2.1 или более поздней версии вызов `Partial` или `RenderPartial` приводит к предупреждению об анализаторе. Например, использование `Partial` выдает следующее предупреждающее сообщение.

> Использование IHtmlHelper.Partial может привести к взаимоблокировкам приложения. Возможно, следует использовать вспомогательную функцию тега `<partial>` или `IHtmlHelper.PartialAsync`.

Замените вызовы к `@Html.Partial` на `@await Html.PartialAsync` или вспомогательную функцию тега частичного представления. Дополнительные сведения о переносе вспомогательной функции тега частичного представления см. в разделе [Перенос из вспомогательного метода HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Обнаружение частичного представления

Ссылаясь на частичное представление, его расположение можно указывать несколькими способами. Пример:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

В предыдущем примере используется вспомогательная функция тега частичного представления, для которой требуется ASP.NET Core 2.1 или более поздней версии. В следующем примере используются асинхронные вспомогательные методы HTML для выполнения той же задачи.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

В разных папках представлений могут быть разные частичные представления с одинаковыми именами. При обращении к представлениям по имени (без указания расширения файла) представление в каждой папке будет использовать частичное представление из той же папки. Можно также определить частичное представление по умолчанию, поместив его в папку *Shared*. Общее частичное представление будет использоваться всеми представлениями, у которых нет собственного частичного представления. Частичное представление по умолчанию (в папке *Shared*) может переопределяться частичным представлением с тем же именем, которое находится в одной папке с родительским представлением.

Частичные представления могут *выстраиваться цепочкой,* &mdash;то есть одно частичное представление может вызывать другое (если при этом не образуется цикл). В каждом обычном или частичном представлении относительные пути всегда указываются относительно данного представления, а не корневого или родительского.

> [!NOTE]
> Объект [Razor](xref:mvc/views/razor) `section`, определенный в частичном представлении, невидим для родительских представлений. Объект `section` видим только для частичного представления, в котором он определен.

## <a name="access-data-from-partial-views"></a>Доступ к данным из частичных представлений

Когда создается экземпляр частичного представления, он получает копию словаря `ViewData` родительского представления. Изменения, вносимые в данные в частичном представлении, не сохраняются в родительском представлении. Изменения объекта `ViewData` в частичном представлении утрачиваются при возврате этого представления.

В частичное представление можно передать экземпляр [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

В частичное представление можно передать модель. Это может быть модель представления страницы или пользовательский объект. Модель можно передать в метод `PartialAsync` или `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

В частичное представление можно передать экземпляр `ViewDataDictionary` и модель представления:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

В приведенной ниже разметке показано представление *Views/Articles/Read.cshtml*, содержащее два частичных представления. Второе частичное представление передает модель и объект `ViewData` в первое частичное представление. Используйте выделенную перегрузку конструктора `ViewDataDictionary`, чтобы передать новый словарь `ViewData` и при этом сохранить существующий словарь `ViewData`.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

Частичное представление *_ArticleSection*

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Во время выполнения частичные представления преобразовываются для просмотра внутри родительского представления, которое само преобразовывается для просмотра в общем макете *_Layout.cshtml*.

![выходные данные частичного представления](partial/_static/output.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end