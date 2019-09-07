---
title: ASP.NET Core макеты Блазор
author: guardrex
description: Узнайте, как создавать многократно используемые компоненты макета для приложений Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/layouts
ms.openlocfilehash: 05a38c10e18407d50422192ab1ddf3ff4b0f3a5b
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800365"
---
# <a name="aspnet-core-blazor-layouts"></a>ASP.NET Core макеты Блазор

[Раинер стропек](https://www.timecockpit.com) и [Люк ЛаСаМ](https://github.com/guardrex)

Некоторые элементы приложения, такие как меню, сообщения об авторских правах и логотипы компании, обычно являются частью общего макета приложения и используются каждым компонентом в приложении. Копирование кода этих элементов во все компоненты приложения не является эффективным подходом&mdash;каждый раз, когда одному из элементов требуется обновление, каждый компонент должен быть обновлен. Такое дублирование сложно поддерживать и может привести к нестабильному содержимому с течением времени. *Разметки* решают эту проблему.

Технически, макет — это просто другой компонент. Макет определяется в шаблоне Razor или в C# коде и может использовать [привязку данных](xref:blazor/components#data-binding), [внедрение зависимостей](xref:blazor/dependency-injection)и сценарии других компонентов.

Чтобы превратить *компонент* в *Макет*, компонент:

* Наследует от `LayoutComponentBase`, который `Body` определяет свойство для отображаемого содержимого внутри макета.
* Использует синтаксис Razor `@Body` для указания расположения в разметке макета, в которой отображается содержимое.

В следующем образце кода показан шаблон Razor компонента макета *маинлайаут. Razor*. Макет наследует `LayoutComponentBase` и `@Body` задает между панелью навигации и нижним колонтитулом:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

В приложении, основанном на одном из шаблонов приложений блазор, `MainLayout` компонент (*маинлайаут. Razor*) находится в *общей* папке приложения.

## <a name="default-layout"></a>Макет по умолчанию

Укажите макет приложения по умолчанию в `Router` компоненте в файле *app. Razor* приложения. Следующий `Router` компонент, предоставляемый шаблонами блазор по умолчанию, задает `MainLayout` для компонента макет по умолчанию:

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

Чтобы предоставить макет `NotFound` по умолчанию для содержимого, `LayoutView` укажите для `NotFound` содержимого:

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

Дополнительные сведения `Router` о компоненте см. в <xref:blazor/routing>разделе.

## <a name="specify-a-layout-in-a-component"></a>Указание макета в компоненте

Используйте директиву `@layout` Razor, чтобы применить макет к компоненту. Компилятор преобразует `@layout` `LayoutAttribute`в, который применяется к классу Component.

Содержимое следующего `MasterList` компонента вставляется в в `MasterLayout` позиции `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a>Выбор централизованного макета

В каждой папке приложения может дополнительно содержаться файл шаблона с именем *_Imports. Razor*. Компилятор включает директивы, указанные в файле импорта, во всех шаблонах Razor в одной и той же папке и рекурсивно во всех ее вложенных папках. Таким образом, файл *_Imports. Razor* , `@layout MyCoolLayout` содержащий, гарантирует, что все компоненты в папке используются `MyCoolLayout`. Нет необходимости повторять добавление `@layout MyCoolLayout` во все файлы *. Razor* в папке и вложенных папках. `@using`директивы также применяются к компонентам таким же образом.

Импортируется следующий файл *_Imports. Razor* :

* `MyCoolLayout`.
* Все компоненты Razor в одной и той же папке и во всех вложенных папках.
* Пространство имен `BlazorApp1.Data` .
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

Файл *_Imports. Razor* аналогичен [файлу _ViewImports. cshtml для представлений и страниц Razor,](xref:mvc/views/layout#importing-shared-directives) но применяется специально к файлам компонентов Razor.

## <a name="nested-layouts"></a>Вложенные макеты

Приложения могут состоять из вложенных макетов. Компонент может ссылаться на макет, который, в свою очередь, ссылается на другой макет. Например, для создания структуры многоуровневого меню используются вложенные макеты.

В следующем примере показано, как использовать вложенные макеты. Файл *еписодескомпонент. Razor* — это компонент для вывода. Компонент ссылается на `MasterListLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

Файл *мастерлистлайаут. Razor* предоставляет `MasterListLayout`. Макет ссылается на другой макет, `MasterLayout`в котором он отображается. `EpisodesComponent`отображается, где `@Body` отображается:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Наконец, `MasterLayout` в *мастерлайаут. Razor* содержатся элементы макета верхнего уровня, такие как заголовок, главное меню и нижний колонтитул. `MasterListLayout``EpisodesComponent` при `@Body` отображении отображается следующее:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/layout>
