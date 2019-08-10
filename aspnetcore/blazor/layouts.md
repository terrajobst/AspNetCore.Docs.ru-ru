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
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="78df8-103">ASP.NET Core макеты Блазор</span><span class="sxs-lookup"><span data-stu-id="78df8-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="78df8-104">По [Раинер стропек](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="78df8-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="78df8-105">Некоторые элементы приложения, такие как меню, сообщения об авторских правах и логотипы компании, обычно являются частью общего макета приложения и используются каждым компонентом в приложении.</span><span class="sxs-lookup"><span data-stu-id="78df8-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="78df8-106">Копирование кода этих элементов во все компоненты приложения не является эффективным подходом&mdash;каждый раз, когда одному из элементов требуется обновление, каждый компонент должен быть обновлен.</span><span class="sxs-lookup"><span data-stu-id="78df8-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="78df8-107">Такое дублирование сложно поддерживать и может привести к нестабильному содержимому с течением времени.</span><span class="sxs-lookup"><span data-stu-id="78df8-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="78df8-108">*Разметки* решают эту проблему.</span><span class="sxs-lookup"><span data-stu-id="78df8-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="78df8-109">Технически, макет — это просто другой компонент.</span><span class="sxs-lookup"><span data-stu-id="78df8-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="78df8-110">Макет определяется в шаблоне Razor или в C# коде и может использовать привязку [данных](xref:blazor/components#data-binding), [внедрение зависимостей](xref:blazor/dependency-injection)и сценарии других компонентов.</span><span class="sxs-lookup"><span data-stu-id="78df8-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="78df8-111">Чтобы превратить *компонент* в *Макет*, компонент:</span><span class="sxs-lookup"><span data-stu-id="78df8-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="78df8-112">Наследует от `LayoutComponentBase`, который `Body` определяет свойство для отображаемого содержимого внутри макета.</span><span class="sxs-lookup"><span data-stu-id="78df8-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="78df8-113">Использует синтаксис Razor `@Body` для указания расположения в разметке макета, в которой отображается содержимое.</span><span class="sxs-lookup"><span data-stu-id="78df8-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="78df8-114">В следующем образце кода показан шаблон Razor компонента макета *маинлайаут. Razor*.</span><span class="sxs-lookup"><span data-stu-id="78df8-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="78df8-115">Макет наследует `LayoutComponentBase` и `@Body` задает между панелью навигации и нижним колонтитулом:</span><span class="sxs-lookup"><span data-stu-id="78df8-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="78df8-116">Указание макета в компоненте</span><span class="sxs-lookup"><span data-stu-id="78df8-116">Specify a layout in a component</span></span>

<span data-ttu-id="78df8-117">Используйте директиву `@layout` Razor, чтобы применить макет к компоненту.</span><span class="sxs-lookup"><span data-stu-id="78df8-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="78df8-118">Компилятор преобразует `@layout` `LayoutAttribute`в, который применяется к классу Component.</span><span class="sxs-lookup"><span data-stu-id="78df8-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="78df8-119">Содержимое следующего компонента, *мастерлист. Razor*, вставляется в в `MainLayout` позиции: `@Body`</span><span class="sxs-lookup"><span data-stu-id="78df8-119">The content of the following component, *MasterList.razor*, is inserted into the `MainLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="78df8-120">Выбор централизованного макета</span><span class="sxs-lookup"><span data-stu-id="78df8-120">Centralized layout selection</span></span>

<span data-ttu-id="78df8-121">В каждой папке приложения может дополнительно содержаться файл шаблона с именем *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="78df8-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="78df8-122">Компилятор включает директивы, указанные в файле импорта, во всех шаблонах Razor в одной и той же папке и рекурсивно во всех ее вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="78df8-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="78df8-123">Таким образом, файл *_Imports. Razor* , `@layout MainLayout` содержащий, гарантирует, что все компоненты в папке используются `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="78df8-123">Therefore, an *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use `MainLayout`.</span></span> <span data-ttu-id="78df8-124">Нет необходимости повторять добавление `@layout MainLayout` во все файлы *. Razor* в папке и вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="78df8-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="78df8-125">`@using`директивы также применяются к компонентам таким же образом.</span><span class="sxs-lookup"><span data-stu-id="78df8-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="78df8-126">Импортируется следующий файл *_Imports. Razor* :</span><span class="sxs-lookup"><span data-stu-id="78df8-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="78df8-127">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="78df8-127">`MainLayout`.</span></span>
* <span data-ttu-id="78df8-128">Все компоненты Razor в одной и той же папке и во всех вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="78df8-128">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="78df8-129">Пространство имен `BlazorApp1.Data` .</span><span class="sxs-lookup"><span data-stu-id="78df8-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="78df8-130">Файл *_Imports. Razor* аналогичен [файлу _ViewImports. cshtml для представлений и страниц Razor,](xref:mvc/views/layout#importing-shared-directives) но применяется специально к файлам компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="78df8-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="78df8-131">Шаблоны Блазор используют файлы *_Imports. Razor* для выбора макета.</span><span class="sxs-lookup"><span data-stu-id="78df8-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="78df8-132">Приложение, созданное из шаблона Блазор, содержит файл *_Imports. Razor* в корне проекта и в папке Pages .</span><span class="sxs-lookup"><span data-stu-id="78df8-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="78df8-133">Вложенные макеты</span><span class="sxs-lookup"><span data-stu-id="78df8-133">Nested layouts</span></span>

<span data-ttu-id="78df8-134">Приложения могут состоять из вложенных макетов.</span><span class="sxs-lookup"><span data-stu-id="78df8-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="78df8-135">Компонент может ссылаться на макет, который, в свою очередь, ссылается на другой макет.</span><span class="sxs-lookup"><span data-stu-id="78df8-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="78df8-136">Например, для создания структуры многоуровневого меню используются вложенные макеты.</span><span class="sxs-lookup"><span data-stu-id="78df8-136">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="78df8-137">В следующем примере показано, как использовать вложенные макеты.</span><span class="sxs-lookup"><span data-stu-id="78df8-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="78df8-138">Файл *еписодескомпонент. Razor* — это компонент для вывода.</span><span class="sxs-lookup"><span data-stu-id="78df8-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="78df8-139">Компонент ссылается на `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="78df8-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="78df8-140">Файл *мастерлистлайаут. Razor* предоставляет `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="78df8-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="78df8-141">Макет ссылается на другой макет, `MasterLayout`в котором он отображается.</span><span class="sxs-lookup"><span data-stu-id="78df8-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="78df8-142">`EpisodesComponent`отображается, где `@Body` отображается:</span><span class="sxs-lookup"><span data-stu-id="78df8-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="78df8-143">Наконец, `MasterLayout` в *мастерлайаут. Razor* содержатся элементы макета верхнего уровня, такие как заголовок, главное меню и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="78df8-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="78df8-144">`MasterListLayout`в `EpisodesComponent` отображаются, где `@Body` отображается:</span><span class="sxs-lookup"><span data-stu-id="78df8-144">`MasterListLayout` with `EpisodesComponent` are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="78df8-145">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78df8-145">Additional resources</span></span>

* <xref:mvc/views/layout>
