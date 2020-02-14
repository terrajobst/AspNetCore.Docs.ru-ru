---
title: ASP.NET Core макеты Blazor
author: guardrex
description: Узнайте, как создавать многократно используемые компоненты макета для Blazor приложений.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 8e7294f6b66d34781473522a71f929ed5f9c33f2
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213380"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a>ASP.NET Core макеты Blazor

[Раинер стропек](https://www.timecockpit.com) и [Люк ЛаСаМ](https://github.com/guardrex)

Некоторые элементы приложения, такие как меню, сообщения об авторских правах и логотипы компании, обычно являются частью общего макета приложения и используются каждым компонентом в приложении. Копирование кода этих элементов во все компоненты приложения не является эффективным подходом&mdash;каждый раз, когда одному из элементов требуется обновление, каждый компонент должен быть обновлен. Такое дублирование сложно поддерживать и может привести к нестабильному содержимому с течением времени. *Разметки* решают эту проблему.

Технически, макет — это просто другой компонент. Макет определяется в шаблоне Razor или в C# коде и может использовать [привязку данных](xref:blazor/components#data-binding), [внедрение зависимостей](xref:blazor/dependency-injection)и сценарии других компонентов.

Чтобы превратить *компонент* в *Макет*, компонент:

* Наследует от `LayoutComponentBase`, который определяет свойство `Body` для отображаемого содержимого внутри макета.
* Использует `@Body` синтаксис Razor, чтобы указать расположение в разметке макета, в которой отображается содержимое.

В следующем образце кода показан шаблон Razor компонента макета *маинлайаут. Razor*. Макет наследует `LayoutComponentBase` и задает `@Body` между панелью навигации и нижним колонтитулом:

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

В приложении, основанном на одном из Blazor шаблонов приложений, компонент `MainLayout` (*маинлайаут. Razor*) находится в *общей* папке приложения.

## <a name="default-layout"></a>Макет по умолчанию

Укажите макет приложения по умолчанию в компоненте `Router` в файле *app. Razor* приложения. Следующий `Router` компонент, предоставляемый шаблонами Blazor по умолчанию, задает для макета по умолчанию компонент `MainLayout`:

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

Чтобы предоставить макет по умолчанию для `NotFound` содержимого, укажите `LayoutView` для `NotFound` содержимого:

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

Дополнительные сведения о компоненте `Router` см. в разделе <xref:blazor/routing>.

Выбор макета в качестве макета по умолчанию в маршрутизаторе является полезной практикой, так как он может быть переопределен отдельно для каждого компонента или папки. Предпочитать использование маршрутизатора для настройки макета приложения по умолчанию, так как это наиболее общий способ.

## <a name="specify-a-layout-in-a-component"></a>Указание макета в компоненте

Используйте директиву Razor `@layout`, чтобы применить макет к компоненту. Компилятор преобразует `@layout` в `LayoutAttribute`, который применяется к классу Component.

Содержимое следующего `MasterList` компонента вставляется в `MasterLayout` в позиции `@Body`:

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

Указание макета непосредственно в компоненте переопределяет набор *макетов по умолчанию* в маршрутизаторе или директиву `@layout`, импортированную из *_Imports. Razor*.

## <a name="centralized-layout-selection"></a>Выбор централизованного макета

В каждой папке приложения может дополнительно содержаться файл шаблона с именем *_Imports. Razor*. Компилятор включает директивы, указанные в файле импорта, во всех шаблонах Razor в одной и той же папке и рекурсивно во всех ее вложенных папках. Таким образом, файл *_Imports. Razor* , содержащий `@layout MyCoolLayout`, гарантирует, что все компоненты в папке используют `MyCoolLayout`. Нет необходимости многократно добавлять `@layout MyCoolLayout` во все файлы *. Razor* в папке и вложенных папках. директивы `@using` также применяются к компонентам таким же образом.

Следующий файл *_Imports. Razor* импортирует:

* `MyCoolLayout`.
* Все компоненты Razor в одной и той же папке и во всех вложенных папках.
* Пространство имен `BlazorApp1.Data`.
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

Файл *_Imports. Razor* аналогичен [файлу _ViewImports. cshtml для представлений и страниц Razor,](xref:mvc/views/layout#importing-shared-directives) но применяется специально к файлам компонентов Razor.

Указание макета в *_Imports. Razor* переопределяет макет, указанный в качестве макета маршрутизатора *по умолчанию*.

## <a name="nested-layouts"></a>Вложенные макеты

Приложения могут состоять из вложенных макетов. Компонент может ссылаться на макет, который, в свою очередь, ссылается на другой макет. Например, для создания структуры многоуровневого меню используются вложенные макеты.

В следующем примере показано, как использовать вложенные макеты. Файл *еписодескомпонент. Razor* — это компонент для вывода. Компонент ссылается на `MasterListLayout`:

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

Файл *мастерлистлайаут. Razor* предоставляет `MasterListLayout`. Макет ссылается на другой макет, `MasterLayout`, где он готовится к просмотру. `EpisodesComponent` отображается, где отображается `@Body`:

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Наконец, `MasterLayout` в *мастерлайаут. Razor* содержит элементы макета верхнего уровня, такие как заголовок, главное меню и нижний колонтитул. `MasterListLayout` с `EpisodesComponent` отображается там, где `@Body` отображается:

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a>Совместное использование макета Razor Pages с интегрированными компонентами

Если маршрутизируемые компоненты интегрированы в приложение Razor Pages, общий макет приложения можно использовать с компонентами. Дополнительные сведения см. в разделе <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/layout>
