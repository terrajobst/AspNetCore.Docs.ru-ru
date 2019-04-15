---
title: Макеты компонентов Razor
author: guardrex
description: Узнайте, как создать компоненты повторно используемый шаблон для Razor компонентов приложений.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515645"
---
# <a name="razor-components-layouts"></a>Макеты компонентов Razor

По [Rainer Stropek](https://www.timecockpit.com)

Приложения обычно содержат более одного компонента. Элементы макета, такие как меню, сообщения об авторских правах и логотипы, должен присутствовать во всех компонентах. Скопировав код из этих элементов макета в все компоненты приложения не является эффективным методом. Такое дублирование сложен в сопровождении и, возможно, приводит к несогласованности содержимое со временем. *Макеты* решить эту проблему.

С технической точки зрения макета — просто еще один компонент. Макет определяется в шаблоне Razor или в C# кода и может содержать [привязки данных](xref:razor-components/components#data-binding), [внедрения зависимостей](xref:razor-components/dependency-injection)и прочих обычных компонентов.

Включить два дополнительных аспектов *компонент* в *макета*

* Макет компонента должен наследовать от `LayoutComponentBase`. `LayoutComponentBase` Определяет `Body` свойство, которое содержит содержимое для отображения внутри макета.
* Использует компонент макета `Body` свойство, чтобы указать, где должен быть содержимому текста к просмотру с использованием синтаксиса Razor `@Body`. Во время отрисовки, `@Body` заменяется содержимое макета.

В следующем образце кода показан шаблон Razor компонента макета. Обратите внимание на использование `LayoutComponentBase` и `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a>Использование макета в компоненте

Использовать директивы Razor `@layout` для применения макета в компонент. Компилятор преобразует эту директиву в `LayoutAttribute`, который применяется к классу component.

В следующем образце кода показано концепции. Содержимое этого компонента будет вставлен *MasterLayout* с позиции `@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>Выбор централизованного макета

Каждой папки из приложения может содержать файл шаблона с именем *_ViewImports.cshtml*. Компилятор включает директивы, указанный в файле imports представление всех шаблонов Razor в той же папке и рекурсивно во всех вложенных в нее папках. Таким образом *_ViewImports.cshtml* файл, содержащий `@layout MainLayout` гарантирует, что все компоненты в папку, используйте *MainLayout* макета. Нет необходимости повторно добавить `@layout` ко всем *.razor* файлов.

Обратите внимание, что в шаблоне по умолчанию используется *_ViewImports.cshtml* механизм для выбора макета. Содержит только что созданному приложению *_ViewImports.cshtml* файл *компоненты/страниц* папки.

## <a name="nested-layouts"></a>Вложенным макетам

Приложения могут состоять из вложенным макетам. Компонент может ссылаться на макет, который в свою очередь ссылается на другую структуру. Например можно использовать вложенности макетов в соответствии с многоуровневой структурой.

В следующем образце кода демонстрируется использование вложенным макетам. *EpisodesComponent.cshtml* файл — это компонент для отображения. Обратите внимание, что компонент ссылается на макет `MasterListLayout`.

*EpisodesComponent.cshtml*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

*MasterListLayout.cshtml* файл предоставляет `MasterListLayout`. Макет ссылается на другую структуру `MasterLayout`, куда они передаются для внедрения.

*MasterListLayout.cshtml*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

Наконец `MasterLayout` содержит элементы макета верхнего уровня, например заголовка, нижнего колонтитула и главного меню.

*MasterLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
