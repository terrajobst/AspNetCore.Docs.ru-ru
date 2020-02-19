---
title: ASP.NET Core Blazor сложных сценариев
author: guardrex
description: Дополнительные сведения о сложных сценариях в Blazor, в том числе о том, как включить логику Рендертрибуилдер вручную в приложение.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453184"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="de26c-103">ASP.NET Core Блазор сложные сценарии</span><span class="sxs-lookup"><span data-stu-id="de26c-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="de26c-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="de26c-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="de26c-105">Логика Рендертрибуилдер вручную</span><span class="sxs-lookup"><span data-stu-id="de26c-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="de26c-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.</span><span class="sxs-lookup"><span data-stu-id="de26c-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="de26c-107">Использование `RenderTreeBuilder` для создания компонентов является расширенным сценарием.</span><span class="sxs-lookup"><span data-stu-id="de26c-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="de26c-108">Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="de26c-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="de26c-109">Рассмотрим следующий компонент `PetDetails`, который можно вручную встроить в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="de26c-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="de26c-110">В следующем примере цикл в методе `CreateComponent` создает три `PetDetails` компонентов.</span><span class="sxs-lookup"><span data-stu-id="de26c-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="de26c-111">При вызове методов `RenderTreeBuilder` для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="de26c-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="de26c-112">Алгоритм разницы Блазор зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова.</span><span class="sxs-lookup"><span data-stu-id="de26c-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="de26c-113">При создании компонента с помощью методов `RenderTreeBuilder` жестко указывайте аргументы для порядковых номеров.</span><span class="sxs-lookup"><span data-stu-id="de26c-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="de26c-114">**Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.**</span><span class="sxs-lookup"><span data-stu-id="de26c-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="de26c-115">Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="de26c-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="de26c-116">`BuiltContent` компонент:</span><span class="sxs-lookup"><span data-stu-id="de26c-116">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="de26c-117">Типы в `Microsoft.AspNetCore.Components.RenderTree` позволяют обрабатывать *результаты* операций отрисовки.</span><span class="sxs-lookup"><span data-stu-id="de26c-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="de26c-118">Это внутренние сведения о реализации Блазор Framework.</span><span class="sxs-lookup"><span data-stu-id="de26c-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="de26c-119">Эти типы следует считать *нестабильными* и могут быть изменены в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="de26c-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="de26c-120">Порядковые номера связаны с номерами строк кода, а не с порядком выполнения</span><span class="sxs-lookup"><span data-stu-id="de26c-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="de26c-121">Файлы компонентов Razor ( *. Razor*) всегда компилируются.</span><span class="sxs-lookup"><span data-stu-id="de26c-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="de26c-122">Компиляция — это потенциальное преимущество для интерпретации кода, поскольку шаг компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="de26c-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="de26c-123">В качестве ключевого примера этих усовершенствований используются *порядковые номера*.</span><span class="sxs-lookup"><span data-stu-id="de26c-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="de26c-124">Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода.</span><span class="sxs-lookup"><span data-stu-id="de26c-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="de26c-125">Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.</span><span class="sxs-lookup"><span data-stu-id="de26c-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="de26c-126">Рассмотрим следующий файл компонента Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="de26c-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="de26c-127">Предыдущий код компилируется примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="de26c-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="de26c-128">Когда код выполняется в первый раз, если `someFlag` `true`, построитель получит следующее:</span><span class="sxs-lookup"><span data-stu-id="de26c-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="de26c-129">Последовательность</span><span class="sxs-lookup"><span data-stu-id="de26c-129">Sequence</span></span> | <span data-ttu-id="de26c-130">Тип</span><span class="sxs-lookup"><span data-stu-id="de26c-130">Type</span></span>      | <span data-ttu-id="de26c-131">Данные</span><span class="sxs-lookup"><span data-stu-id="de26c-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="de26c-132">0</span><span class="sxs-lookup"><span data-stu-id="de26c-132">0</span></span>        | <span data-ttu-id="de26c-133">Узел Text</span><span class="sxs-lookup"><span data-stu-id="de26c-133">Text node</span></span> | <span data-ttu-id="de26c-134">Первый</span><span class="sxs-lookup"><span data-stu-id="de26c-134">First</span></span>  |
| <span data-ttu-id="de26c-135">1</span><span class="sxs-lookup"><span data-stu-id="de26c-135">1</span></span>        | <span data-ttu-id="de26c-136">Узел Text</span><span class="sxs-lookup"><span data-stu-id="de26c-136">Text node</span></span> | <span data-ttu-id="de26c-137">Секунда</span><span class="sxs-lookup"><span data-stu-id="de26c-137">Second</span></span> |

<span data-ttu-id="de26c-138">Представьте, что `someFlag` становится `false`, и разметка снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="de26c-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="de26c-139">На этот раз построитель получит:</span><span class="sxs-lookup"><span data-stu-id="de26c-139">This time, the builder receives:</span></span>

| <span data-ttu-id="de26c-140">Последовательность</span><span class="sxs-lookup"><span data-stu-id="de26c-140">Sequence</span></span> | <span data-ttu-id="de26c-141">Тип</span><span class="sxs-lookup"><span data-stu-id="de26c-141">Type</span></span>       | <span data-ttu-id="de26c-142">Данные</span><span class="sxs-lookup"><span data-stu-id="de26c-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="de26c-143">1</span><span class="sxs-lookup"><span data-stu-id="de26c-143">1</span></span>        | <span data-ttu-id="de26c-144">Узел Text</span><span class="sxs-lookup"><span data-stu-id="de26c-144">Text node</span></span>  | <span data-ttu-id="de26c-145">Секунда</span><span class="sxs-lookup"><span data-stu-id="de26c-145">Second</span></span> |

<span data-ttu-id="de26c-146">Когда среда выполнения выполняет поиск различий, она видит, что элемент в последовательности `0` был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:</span><span class="sxs-lookup"><span data-stu-id="de26c-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="de26c-147">Удалите первый текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="de26c-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="de26c-148">Проблема с созданием порядковых номеров программным способом</span><span class="sxs-lookup"><span data-stu-id="de26c-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="de26c-149">Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:</span><span class="sxs-lookup"><span data-stu-id="de26c-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="de26c-150">Теперь первые выходные данные:</span><span class="sxs-lookup"><span data-stu-id="de26c-150">Now, the first output is:</span></span>

| <span data-ttu-id="de26c-151">Последовательность</span><span class="sxs-lookup"><span data-stu-id="de26c-151">Sequence</span></span> | <span data-ttu-id="de26c-152">Тип</span><span class="sxs-lookup"><span data-stu-id="de26c-152">Type</span></span>      | <span data-ttu-id="de26c-153">Данные</span><span class="sxs-lookup"><span data-stu-id="de26c-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="de26c-154">0</span><span class="sxs-lookup"><span data-stu-id="de26c-154">0</span></span>        | <span data-ttu-id="de26c-155">Узел Text</span><span class="sxs-lookup"><span data-stu-id="de26c-155">Text node</span></span> | <span data-ttu-id="de26c-156">Первый</span><span class="sxs-lookup"><span data-stu-id="de26c-156">First</span></span>  |
| <span data-ttu-id="de26c-157">1</span><span class="sxs-lookup"><span data-stu-id="de26c-157">1</span></span>        | <span data-ttu-id="de26c-158">Узел Text</span><span class="sxs-lookup"><span data-stu-id="de26c-158">Text node</span></span> | <span data-ttu-id="de26c-159">Секунда</span><span class="sxs-lookup"><span data-stu-id="de26c-159">Second</span></span> |

<span data-ttu-id="de26c-160">Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают.</span><span class="sxs-lookup"><span data-stu-id="de26c-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="de26c-161">во второй отрисовке `someFlag` `false`, а выходные данные:</span><span class="sxs-lookup"><span data-stu-id="de26c-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="de26c-162">Последовательность</span><span class="sxs-lookup"><span data-stu-id="de26c-162">Sequence</span></span> | <span data-ttu-id="de26c-163">Тип</span><span class="sxs-lookup"><span data-stu-id="de26c-163">Type</span></span>      | <span data-ttu-id="de26c-164">Данные</span><span class="sxs-lookup"><span data-stu-id="de26c-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="de26c-165">0</span><span class="sxs-lookup"><span data-stu-id="de26c-165">0</span></span>        | <span data-ttu-id="de26c-166">Узел Text</span><span class="sxs-lookup"><span data-stu-id="de26c-166">Text node</span></span> | <span data-ttu-id="de26c-167">Секунда</span><span class="sxs-lookup"><span data-stu-id="de26c-167">Second</span></span> |

<span data-ttu-id="de26c-168">На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:</span><span class="sxs-lookup"><span data-stu-id="de26c-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="de26c-169">Измените значение первого текстового узла на `Second`.</span><span class="sxs-lookup"><span data-stu-id="de26c-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="de26c-170">Удалите второй текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="de26c-170">Remove the second text node.</span></span>

<span data-ttu-id="de26c-171">При формировании порядковых номеров теряются все полезные сведения о том, где в исходном коде находятся `if/else` ветви и циклы.</span><span class="sxs-lookup"><span data-stu-id="de26c-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="de26c-172">Это приводит к **удвоению в два раза больше** , чем раньше.</span><span class="sxs-lookup"><span data-stu-id="de26c-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="de26c-173">Это тривиальный пример.</span><span class="sxs-lookup"><span data-stu-id="de26c-173">This is a trivial example.</span></span> <span data-ttu-id="de26c-174">В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность обычно выше.</span><span class="sxs-lookup"><span data-stu-id="de26c-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="de26c-175">Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен выполнять рекурсивный глубокий переход к деревьям отрисовки.</span><span class="sxs-lookup"><span data-stu-id="de26c-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="de26c-176">Это, как правило, приводит к более длительному изменению скриптов, поскольку алгоритм diff сообщает о том, как старые и новые структуры связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="de26c-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="de26c-177">Руководство и выводы</span><span class="sxs-lookup"><span data-stu-id="de26c-177">Guidance and conclusions</span></span>

* <span data-ttu-id="de26c-178">Производительность приложения снижается, если порядковые номера создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="de26c-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="de26c-179">Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="de26c-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="de26c-180">Не записывайте длинные блоки `RenderTreeBuilder` логики, реализуемой вручную.</span><span class="sxs-lookup"><span data-stu-id="de26c-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="de26c-181">Предпочитать файлы *Razor* и позволяют компилятору работать с порядковыми номерами.</span><span class="sxs-lookup"><span data-stu-id="de26c-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="de26c-182">Если не удается избежать ручного `RenderTreeBuilder` логики, разделите длинные блоки кода на более мелкие части, заключенные в `OpenRegion`/`CloseRegion` вызовы.</span><span class="sxs-lookup"><span data-stu-id="de26c-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="de26c-183">Каждый регион имеет собственное отдельное пространство порядковых номеров, поэтому вы можете перезапускаться от нуля (или любого другого произвольного числа) внутри каждого региона.</span><span class="sxs-lookup"><span data-stu-id="de26c-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="de26c-184">Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении.</span><span class="sxs-lookup"><span data-stu-id="de26c-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="de26c-185">Начальное значение и зазоры несущественны.</span><span class="sxs-lookup"><span data-stu-id="de26c-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="de26c-186">Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал).</span><span class="sxs-lookup"><span data-stu-id="de26c-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="de26c-187"> использует порядковые номера, в то время как другие платформы, использующие средства различения дерева, не используют их.</span><span class="sxs-lookup"><span data-stu-id="de26c-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="de26c-188">Сравнение выполняется гораздо быстрее, если используются порядковые номера, и Blazor имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков, занимающихся созданием файлов *Razor* .</span><span class="sxs-lookup"><span data-stu-id="de26c-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
