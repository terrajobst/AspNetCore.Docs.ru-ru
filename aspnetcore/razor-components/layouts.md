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
# <a name="razor-components-layouts"></a><span data-ttu-id="e7eb7-103">Макеты компонентов Razor</span><span class="sxs-lookup"><span data-stu-id="e7eb7-103">Razor Components layouts</span></span>

<span data-ttu-id="e7eb7-104">По [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="e7eb7-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="e7eb7-105">Приложения обычно содержат более одного компонента.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="e7eb7-106">Элементы макета, такие как меню, сообщения об авторских правах и логотипы, должен присутствовать во всех компонентах.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="e7eb7-107">Скопировав код из этих элементов макета в все компоненты приложения не является эффективным методом.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="e7eb7-108">Такое дублирование сложен в сопровождении и, возможно, приводит к несогласованности содержимое со временем.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="e7eb7-109">*Макеты* решить эту проблему.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="e7eb7-110">С технической точки зрения макета — просто еще один компонент.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="e7eb7-111">Макет определяется в шаблоне Razor или в C# кода и может содержать [привязки данных](xref:razor-components/components#data-binding), [внедрения зависимостей](xref:razor-components/dependency-injection)и прочих обычных компонентов.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="e7eb7-112">Включить два дополнительных аспектов *компонент* в *макета*</span><span class="sxs-lookup"><span data-stu-id="e7eb7-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="e7eb7-113">Макет компонента должен наследовать от `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-113">The layout component must inherit from `LayoutComponentBase`.</span></span> `LayoutComponentBase` <span data-ttu-id="e7eb7-114">Определяет `Body` свойство, которое содержит содержимое для отображения внутри макета.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-114">defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="e7eb7-115">Использует компонент макета `Body` свойство, чтобы указать, где должен быть содержимому текста к просмотру с использованием синтаксиса Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="e7eb7-116">Во время отрисовки, `@Body` заменяется содержимое макета.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="e7eb7-117">В следующем образце кода показан шаблон Razor компонента макета.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="e7eb7-118">Обратите внимание на использование `LayoutComponentBase` и `@Body`:</span><span class="sxs-lookup"><span data-stu-id="e7eb7-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="e7eb7-119">Использование макета в компоненте</span><span class="sxs-lookup"><span data-stu-id="e7eb7-119">Use a layout in a component</span></span>

<span data-ttu-id="e7eb7-120">Использовать директивы Razor `@layout` для применения макета в компонент.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="e7eb7-121">Компилятор преобразует эту директиву в `LayoutAttribute`, который применяется к классу component.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="e7eb7-122">В следующем образце кода показано концепции.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="e7eb7-123">Содержимое этого компонента будет вставлен *MasterLayout* с позиции `@Body`:</span><span class="sxs-lookup"><span data-stu-id="e7eb7-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="e7eb7-124">Выбор централизованного макета</span><span class="sxs-lookup"><span data-stu-id="e7eb7-124">Centralized layout selection</span></span>

<span data-ttu-id="e7eb7-125">Каждой папки из приложения может содержать файл шаблона с именем *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="e7eb7-126">Компилятор включает директивы, указанный в файле imports представление всех шаблонов Razor в той же папке и рекурсивно во всех вложенных в нее папках.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="e7eb7-127">Таким образом *_ViewImports.cshtml* файл, содержащий `@layout MainLayout` гарантирует, что все компоненты в папку, используйте *MainLayout* макета.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="e7eb7-128">Нет необходимости повторно добавить `@layout` ко всем *.razor* файлов.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="e7eb7-129">Обратите внимание, что в шаблоне по умолчанию используется *_ViewImports.cshtml* механизм для выбора макета.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="e7eb7-130">Содержит только что созданному приложению *_ViewImports.cshtml* файл *компоненты/страниц* папки.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="e7eb7-131">Вложенным макетам</span><span class="sxs-lookup"><span data-stu-id="e7eb7-131">Nested layouts</span></span>

<span data-ttu-id="e7eb7-132">Приложения могут состоять из вложенным макетам.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="e7eb7-133">Компонент может ссылаться на макет, который в свою очередь ссылается на другую структуру.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="e7eb7-134">Например можно использовать вложенности макетов в соответствии с многоуровневой структурой.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="e7eb7-135">В следующем образце кода демонстрируется использование вложенным макетам.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="e7eb7-136">*EpisodesComponent.cshtml* файл — это компонент для отображения.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="e7eb7-137">Обратите внимание, что компонент ссылается на макет `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="e7eb7-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7eb7-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="e7eb7-139">*MasterListLayout.cshtml* файл предоставляет `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="e7eb7-140">Макет ссылается на другую структуру `MasterLayout`, куда они передаются для внедрения.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="e7eb7-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7eb7-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="e7eb7-142">Наконец `MasterLayout` содержит элементы макета верхнего уровня, например заголовка, нижнего колонтитула и главного меню.</span><span class="sxs-lookup"><span data-stu-id="e7eb7-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="e7eb7-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7eb7-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
