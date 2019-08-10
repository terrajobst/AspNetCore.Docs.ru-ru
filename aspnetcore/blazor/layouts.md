---
title: ASP.NET Core макеты Блазор
author: guardrex
description: Узнайте, как создавать многократно используемые компоненты макета для приложений Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/layouts
ms.openlocfilehash: 2d652e149381f0a93e3135da978ab5737d47c6f1
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "68948224"
---
# <a name="aspnet-core-blazor-layouts"></a>ASP.NET Core макеты Блазор

По [Раинер стропек](https://www.timecockpit.com)

Некоторые элементы приложения, такие как меню, сообщения об авторских правах и логотипы компании, обычно являются частью общего макета приложения и используются каждым компонентом в приложении. Копирование кода этих элементов во все компоненты приложения не является эффективным подходом&mdash;каждый раз, когда одному из элементов требуется обновление, каждый компонент должен быть обновлен. Такое дублирование сложно поддерживать и может привести к нестабильному содержимому с течением времени. *Разметки* решают эту проблему.

Технически, макет — это просто другой компонент. Макет определяется в шаблоне Razor или в C# коде и может использовать привязку [данных](xref:blazor/components#data-binding), [внедрение зависимостей](xref:blazor/dependency-injection)и сценарии других компонентов.

Чтобы превратить *компонент* в *Макет*, компонент:

* Наследует от `LayoutComponentBase`, который `Body` определяет свойство для отображаемого содержимого внутри макета.
* Использует синтаксис Razor `@Body` для указания расположения в разметке макета, в которой отображается содержимое.

В следующем образце кода показан шаблон Razor компонента макета *маинлайаут. Razor*. Макет наследует `LayoutComponentBase` и `@Body` задает между панелью навигации и нижним колонтитулом:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a>Указание макета в компоненте

Используйте директиву `@layout` Razor, чтобы применить макет к компоненту. Компилятор преобразует `@layout` `LayoutAttribute`в, который применяется к классу Component.

Содержимое следующего компонента, *мастерлист. Razor*, вставляется в в `MainLayout` позиции: `@Body`

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a>Выбор централизованного макета

В каждой папке приложения может дополнительно содержаться файл шаблона с именем *_Imports. Razor*. Компилятор включает директивы, указанные в файле импорта, во всех шаблонах Razor в одной и той же папке и рекурсивно во всех ее вложенных папках. Таким образом, файл *_Imports. Razor* , `@layout MainLayout` содержащий, гарантирует, что все компоненты в папке используются `MainLayout`. Нет необходимости повторять добавление `@layout MainLayout` во все файлы *. Razor* в папке и вложенных папках. `@using`директивы также применяются к компонентам таким же образом.

Импортируется следующий файл *_Imports. Razor* :

* `MainLayout`.
* Все компоненты Razor в одной и той же папке и во всех вложенных папках.
* Пространство имен `BlazorApp1.Data` .
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

Файл *_Imports. Razor* аналогичен [файлу _ViewImports. cshtml для представлений и страниц Razor,](xref:mvc/views/layout#importing-shared-directives) но применяется специально к файлам компонентов Razor.

Шаблоны Блазор используют файлы *_Imports. Razor* для выбора макета. Приложение, созданное из шаблона Блазор, содержит файл *_Imports. Razor* в корне проекта и в папке Pages .

## <a name="nested-layouts"></a>Вложенные макеты

Приложения могут состоять из вложенных макетов. Компонент может ссылаться на макет, который, в свою очередь, ссылается на другой макет. Например, для создания структуры многоуровневого меню используются вложенные макеты.

В следующем примере показано, как использовать вложенные макеты. Файл *еписодескомпонент. Razor* — это компонент для вывода. Компонент ссылается на `MasterListLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

Файл *мастерлистлайаут. Razor* предоставляет `MasterListLayout`. Макет ссылается на другой макет, `MasterLayout`в котором он отображается. `EpisodesComponent`отображается, где `@Body` отображается:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Наконец, `MasterLayout` в *мастерлайаут. Razor* содержатся элементы макета верхнего уровня, такие как заголовок, главное меню и нижний колонтитул. `MasterListLayout`в `EpisodesComponent` отображаются, где `@Body` отображается:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/layout>
