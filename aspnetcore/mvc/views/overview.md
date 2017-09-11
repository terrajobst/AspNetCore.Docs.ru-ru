---
title: "Общие сведения о представлениях"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 7abfa7ef855eb95e1a27ba6a699dd923c9e4d7c0
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="b1eac-103">Визуализации HTML с представлениями в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b1eac-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="b1eac-104">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="b1eac-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="b1eac-105">Контроллеры ASP.NET Core MVC может возвращать форматированные результатов с помощью *представления*.</span><span class="sxs-lookup"><span data-stu-id="b1eac-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="b1eac-106">Что такое представления</span><span class="sxs-lookup"><span data-stu-id="b1eac-106">What are Views?</span></span>

<span data-ttu-id="b1eac-107">В шаблон Model-View-Controller (MVC) *представление* инкапсулирует данные представления взаимодействие пользователя с приложением.</span><span class="sxs-lookup"><span data-stu-id="b1eac-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="b1eac-108">Представления — это шаблоны HTML с внедренным кодом, формирующих содержимое, отправляемое клиенту.</span><span class="sxs-lookup"><span data-stu-id="b1eac-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="b1eac-109">Представлениях используется [синтаксисом Razor](razor.md), что позволяет коду взаимодействовать с HTML с помощью минимального программного кода или процедуры.</span><span class="sxs-lookup"><span data-stu-id="b1eac-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="b1eac-110">Представления ASP.NET Core MVC являются *.cshtml* файлы, хранящиеся в по умолчанию *представления* папки в приложении.</span><span class="sxs-lookup"><span data-stu-id="b1eac-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="b1eac-111">Как правило каждый контроллер будет содержать собственной папке, в которой являются для конкретного контроллера действия.</span><span class="sxs-lookup"><span data-stu-id="b1eac-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![Папка Views в обозревателе решений](overview/_static/views_solution_explorer.png)

<span data-ttu-id="b1eac-113">Помимо представления специфические для действия [разделяемые представления](partial.md), [макеты и другие файлы специальное представление](layout.md) позволяет сократить повторения и разрешить для повторного использования в пределах представления приложения.</span><span class="sxs-lookup"><span data-stu-id="b1eac-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="b1eac-114">Преимущества использования представлений</span><span class="sxs-lookup"><span data-stu-id="b1eac-114">Benefits of Using Views</span></span>

<span data-ttu-id="b1eac-115">Представления предоставляют [Разделение областей ответственности](http://deviq.com/separation-of-concerns/) в приложении MVC, инкапсулируя разметки уровня пользовательского интерфейса отдельно от бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="b1eac-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="b1eac-116">ASP.NET MVC представлениях используется [синтаксисом Razor](razor.md) на переключение между HTML разметку и серверные стороны логику мышки.</span><span class="sxs-lookup"><span data-stu-id="b1eac-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="b1eac-117">Общие и повторяющиеся аспектов пользовательского интерфейса приложения можно легко использовать повторно между режимами с помощью [макет и директивы общего](layout.md) или [разделяемые представления](partial.md).</span><span class="sxs-lookup"><span data-stu-id="b1eac-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="b1eac-118">Создание представления</span><span class="sxs-lookup"><span data-stu-id="b1eac-118">Creating a View</span></span>

<span data-ttu-id="b1eac-119">Представления, относящиеся к контроллеру создаются в *представления / [Имя_контроллера]* папки.</span><span class="sxs-lookup"><span data-stu-id="b1eac-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="b1eac-120">Представления, которые являются общими для контроллеров, помещаются в */представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="b1eac-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="b1eac-121">Назовите файл представление таким же, как его соответствующему контроллеру действие и добавить *.cshtml* расширение имени файла.</span><span class="sxs-lookup"><span data-stu-id="b1eac-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="b1eac-122">Например, чтобы создать представление для *о* действие на *Главная* контроллера, необходимо создать *About.cshtml* файла в  * /представления/домашние*папки.</span><span class="sxs-lookup"><span data-stu-id="b1eac-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="b1eac-123">Образец файла представления (*About.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="b1eac-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="b1eac-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b1eac-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="b1eac-125">*Razor* обозначается код `@` символов.</span><span class="sxs-lookup"><span data-stu-id="b1eac-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="b1eac-126">Операторы C#, выполняются в Razor, отключить блоки кода в фигурные скобки (`{` `}`), такие как назначение «О программе» для `ViewData["Title"]` элемент, показанный выше.</span><span class="sxs-lookup"><span data-stu-id="b1eac-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="b1eac-127">Razor можно использовать для отображения значений в HTML, просто ссылаясь на значение с `@` symbol, как показано в `<h2>` и `<h3>` элементов, описанных выше.</span><span class="sxs-lookup"><span data-stu-id="b1eac-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="b1eac-128">В этом представлении основное внимание уделяется только часть выходных данных, для которого оно отвечает.</span><span class="sxs-lookup"><span data-stu-id="b1eac-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="b1eac-129">Остальная часть макет страницы и другие общие аспекты представления, указываются в другом месте.</span><span class="sxs-lookup"><span data-stu-id="b1eac-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="b1eac-130">Дополнительные сведения о [макет и логики общего представления](layout.md).</span><span class="sxs-lookup"><span data-stu-id="b1eac-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="b1eac-131">Как сделать контроллеров определение представлений?</span><span class="sxs-lookup"><span data-stu-id="b1eac-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="b1eac-132">Представления обычно возвращаются из действия, как `ViewResult`.</span><span class="sxs-lookup"><span data-stu-id="b1eac-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="b1eac-133">Можно создавать и возвращать метод действия `ViewResult` непосредственно, но чаще Если контроллер наследует от `Controller`, просто используем `View` вспомогательный метод, как в этом примере показано:</span><span class="sxs-lookup"><span data-stu-id="b1eac-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="b1eac-134">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="b1eac-134">*HomeController.cs*</span></span>

<span data-ttu-id="b1eac-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="b1eac-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="b1eac-136">`View` Вспомогательный метод имеет несколько перегрузок, чтобы упростить возвращение представления для разработчиков приложений.</span><span class="sxs-lookup"><span data-stu-id="b1eac-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="b1eac-137">При необходимости можно указать представлению для возврата, а также объект модели для передачи в представление.</span><span class="sxs-lookup"><span data-stu-id="b1eac-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="b1eac-138">По возвращении из этого действия *About.cshtml* визуализации представления, показанный выше:</span><span class="sxs-lookup"><span data-stu-id="b1eac-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![О странице](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="b1eac-140">Представления обнаружения</span><span class="sxs-lookup"><span data-stu-id="b1eac-140">View Discovery</span></span>

<span data-ttu-id="b1eac-141">Если действие возвращает представление, называется *представление обнаружения* имеет место.</span><span class="sxs-lookup"><span data-stu-id="b1eac-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="b1eac-142">Этот процесс определяет, какой файл представления будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="b1eac-142">This process determines which view file will be used.</span></span> <span data-ttu-id="b1eac-143">Если не указан файл определенных представлений, среда выполнения ищет представления для контроллера во-первых, затем выполняется поиск соответствующего имени представления в *Shared* папки.</span><span class="sxs-lookup"><span data-stu-id="b1eac-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="b1eac-144">Если действие возвращает `View` метод, вот так `return View();`, имя действия используется в качестве имени представления.</span><span class="sxs-lookup"><span data-stu-id="b1eac-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="b1eac-145">Например если это были вызывается из метода действия с именем «Индекс», было бы эквивалентно передав имя представления «Индекс».</span><span class="sxs-lookup"><span data-stu-id="b1eac-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="b1eac-146">Имя представления можно явно передать в метод (`return View("SomeView");`).</span><span class="sxs-lookup"><span data-stu-id="b1eac-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="b1eac-147">В обоих случаях представление обнаружения выполняет поиск в файл сопоставления представления:</span><span class="sxs-lookup"><span data-stu-id="b1eac-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="b1eac-148">Представления или\<Имя_контроллера > /\<ViewName > .cshtml</span><span class="sxs-lookup"><span data-stu-id="b1eac-148">Views/\<ControllerName>/\<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="b1eac-149">Представления/Общие/\<ViewName > .cshtml</span><span class="sxs-lookup"><span data-stu-id="b1eac-149">Views/Shared/\<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="b1eac-150">Рекомендуется следовать соглашению простого возврата `View()` из действий, если это возможно, поскольку приводит более гибкий и проще рефакторинга кода.</span><span class="sxs-lookup"><span data-stu-id="b1eac-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="b1eac-151">Путь к файлу представления могут быть предоставлены вместо имени представления.</span><span class="sxs-lookup"><span data-stu-id="b1eac-151">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="b1eac-152">Если с помощью абсолютный путь, начиная с корневого каталога приложения (при необходимости, начиная с «/» или «~ /»), *.cshtml* расширение должно быть указано как часть пути к файлу (например, `return View("Views/Home/About.cshtml");`).</span><span class="sxs-lookup"><span data-stu-id="b1eac-152">If using an absolute path starting at the application root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified as part of the file path (for example, `return View("Views/Home/About.cshtml");`).</span></span> <span data-ttu-id="b1eac-153">Кроме того, можно использовать относительный путь от каталога конкретного контроллера в *представления* каталога для указания представления в разных каталогах (например, `return View("../Manage/Index");` внутри `HomeController`).</span><span class="sxs-lookup"><span data-stu-id="b1eac-153">Alternatively, you can use a relative path from the controller-specific directory within the *Views* directory to specify views in different directories (for example, `return View("../Manage/Index");` inside the `HomeController`).</span></span> <span data-ttu-id="b1eac-154">Аналогичным образом, вы сможете просматривать текущий каталог конкретного контроллера (например, `return View("./About");`).</span><span class="sxs-lookup"><span data-stu-id="b1eac-154">Similarly, you can traverse the current controller-specific directory (for example, `return View("./About");`).</span></span> <span data-ttu-id="b1eac-155">Обратите внимание, что относительные пути не использовать *.cshtml* расширения.</span><span class="sxs-lookup"><span data-stu-id="b1eac-155">Note that relative paths don't use the *.cshtml* extension.</span></span> <span data-ttu-id="b1eac-156">Как упоминалось ранее следуйте рекомендациям по организации структуры файлов для представления для отображения связей между контроллерами, действий и представления для удобства и простоты.</span><span class="sxs-lookup"><span data-stu-id="b1eac-156">As previously mentioned, follow the best practice of organizing the file structure for views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

> [!NOTE]
> <span data-ttu-id="b1eac-157">[Разделяемые представления](partial.md) и [просматривать компоненты](view-components.md) использовать механизмы обнаружения, аналогично (но не идентичен).</span><span class="sxs-lookup"><span data-stu-id="b1eac-157">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="b1eac-158">Вы можете настроить соглашение по умолчанию в отношении представления находятся в приложении с помощью пользовательского `IViewLocationExpander`.</span><span class="sxs-lookup"><span data-stu-id="b1eac-158">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="b1eac-159">Имена представлений может учитываться регистр в зависимости от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="b1eac-159">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="b1eac-160">Для совместимости с разными операционными системами всегда учитывать регистр между контроллером и имена действий и связанного представления папки и имен файлов.</span><span class="sxs-lookup"><span data-stu-id="b1eac-160">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="b1eac-161">Передача данных с представлениями</span><span class="sxs-lookup"><span data-stu-id="b1eac-161">Passing Data to Views</span></span>

<span data-ttu-id="b1eac-162">Можно передавать данные представления, используя несколько механизмов.</span><span class="sxs-lookup"><span data-stu-id="b1eac-162">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="b1eac-163">Наиболее надежный подход — указать *модель* типа в представлении (часто называют *viewmodel*, чтобы отличить его от бизнес-типов модели домена), а затем передать экземпляр этого типа в представлении из действия.</span><span class="sxs-lookup"><span data-stu-id="b1eac-163">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="b1eac-164">Мы рекомендуем использовать модель или модель представлений для передачи данных в представлении.</span><span class="sxs-lookup"><span data-stu-id="b1eac-164">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="b1eac-165">Это позволяет воспользоваться преимуществами проверки строгого типа представления.</span><span class="sxs-lookup"><span data-stu-id="b1eac-165">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="b1eac-166">Можно указать модель для представления с помощью `@model` директиву:</span><span class="sxs-lookup"><span data-stu-id="b1eac-166">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="b1eac-167">После модели был определен для представления, переданы в представление экземпляра может осуществляться в режиме строгой типизации с помощью `@Model` как показано выше.</span><span class="sxs-lookup"><span data-stu-id="b1eac-167">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="b1eac-168">Чтобы предоставить экземпляр типа модели к представлению, контроллер передает его в качестве параметра:</span><span class="sxs-lookup"><span data-stu-id="b1eac-168">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
public IActionResult Contact()
   {
       ViewData["Message"] = "Your contact page.";

       var viewModel = new Address()
       {
           Name = "Microsoft",
           Street = "One Microsoft Way",
           City = "Redmond",
           State = "WA",
           PostalCode = "98052-6399"
       };
       return View(viewModel);
   }
   ```

<span data-ttu-id="b1eac-169">Существуют ограничения на типы, которые могут быть предоставлены для представления в виде модели.</span><span class="sxs-lookup"><span data-stu-id="b1eac-169">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="b1eac-170">Корпорация Майкрософт рекомендует передачи простых старых объектов POCO (CLR) Просмотр моделей с минимумом или совсем без поведение, чтобы бизнес-логику можно инкапсулировать в другом месте в приложении.</span><span class="sxs-lookup"><span data-stu-id="b1eac-170">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="b1eac-171">Примером такого подхода является *адрес* viewmodel, используемый в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="b1eac-171">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
namespace WebApplication1.ViewModels
   {
       public class Address
       {
           public string Name { get; set; }
           public string Street { get; set; }
           public string City { get; set; }
           public string State { get; set; }
           public string PostalCode { get; set; }
       }
   }
   ```

> [!NOTE]
> <span data-ttu-id="b1eac-172">Ничто не мешает использовать те же классы в качестве модели вашей бизнес-типов и типов модели экрана.</span><span class="sxs-lookup"><span data-stu-id="b1eac-172">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="b1eac-173">Тем не менее, их позволяет вашим представлениям отличаться независимо от модели домена или сохраняемость, а можно предлагать некоторые преимущества безопасности (для моделей, которые пользователи будут отправлять в приложения с помощью [привязки модели](../models/model-binding.md)).</span><span class="sxs-lookup"><span data-stu-id="b1eac-173">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="b1eac-174">Слабо типизированных данных</span><span class="sxs-lookup"><span data-stu-id="b1eac-174">Loosely Typed Data</span></span>

<span data-ttu-id="b1eac-175">Помимо строго типизированные представления все представления имеют доступ к слабо типизированный набор данных.</span><span class="sxs-lookup"><span data-stu-id="b1eac-175">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="b1eac-176">Этот же коллекции можно указать ссылкой через либо `ViewData` или `ViewBag` свойства для контроллеров и представлений.</span><span class="sxs-lookup"><span data-stu-id="b1eac-176">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="b1eac-177">`ViewBag` Свойство является оболочкой вокруг `ViewData` , предоставляет динамического представления этой коллекции.</span><span class="sxs-lookup"><span data-stu-id="b1eac-177">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="b1eac-178">Не является отдельной коллекции.</span><span class="sxs-lookup"><span data-stu-id="b1eac-178">It is not a separate collection.</span></span>

<span data-ttu-id="b1eac-179">`ViewData`объект словаря осуществляется с помощью `string` ключей.</span><span class="sxs-lookup"><span data-stu-id="b1eac-179">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="b1eac-180">Можно сохранять и извлекать объекты в ней, и необходимо привести их к определенному типу после их извлечения.</span><span class="sxs-lookup"><span data-stu-id="b1eac-180">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="b1eac-181">Можно использовать `ViewData` для передачи данных от контроллера к представлениям, а также представления (разделяемые представления и макеты).</span><span class="sxs-lookup"><span data-stu-id="b1eac-181">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="b1eac-182">Строковые данные могут храниться и использоваться непосредственно, без необходимости для приведения к типу.</span><span class="sxs-lookup"><span data-stu-id="b1eac-182">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="b1eac-183">Некоторые значения для `ViewData` в действии:</span><span class="sxs-lookup"><span data-stu-id="b1eac-183">Set some values for `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
   {
       ViewData["Greeting"] = "Hello";
       ViewData["Address"]  = new Address()
       {
           Name = "Steve",
           Street = "123 Main St",
           City = "Hudson",
           State = "OH",
           PostalCode = "44236"
       };

       return View();
   }
   ```

<span data-ttu-id="b1eac-184">Работать с данными в представлении:</span><span class="sxs-lookup"><span data-stu-id="b1eac-184">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="b1eac-185">`ViewBag` Объектов предоставляет динамический доступ для объектов, хранящихся в `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="b1eac-185">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="b1eac-186">Это может быть удобнее работать с, поскольку он не требует приведения.</span><span class="sxs-lookup"><span data-stu-id="b1eac-186">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="b1eac-187">Тот же пример, как и выше, с помощью `ViewBag` вместо со строгой типизацией `address` экземпляра в представлении:</span><span class="sxs-lookup"><span data-stu-id="b1eac-187">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="b1eac-188">Поскольку оба будут ссылаться на тот же базовый `ViewData` коллекции, можно комбинировать и совпадают `ViewData` и `ViewBag` при чтении и записи значения, если удобно.</span><span class="sxs-lookup"><span data-stu-id="b1eac-188">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="b1eac-189">Динамические представления</span><span class="sxs-lookup"><span data-stu-id="b1eac-189">Dynamic Views</span></span>

<span data-ttu-id="b1eac-190">Представления, которые не объявлять тип модели, но отсутствует экземпляр модели, передаваемых в них может ссылаться динамически данного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="b1eac-190">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="b1eac-191">Например, если экземпляр `Address` передается в представление, которое не объявляет `@model`, по-прежнему будет может ссылаться на свойства этого экземпляра, динамически, как показано представление:</span><span class="sxs-lookup"><span data-stu-id="b1eac-191">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="b1eac-192">Этот компонент можно предлагать некоторую гибкость, но не предоставляет все защиты компиляции или IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="b1eac-192">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="b1eac-193">Если свойство не существует, страница не пройдет во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="b1eac-193">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="b1eac-194">Дополнительные возможности представления</span><span class="sxs-lookup"><span data-stu-id="b1eac-194">More View Features</span></span>

<span data-ttu-id="b1eac-195">[Вспомогательных функций тегов](tag-helpers/intro.md) облегчить добавление поведения на стороне сервера в существующий HTML-тегов, устраняя необходимость использовать пользовательский код или вспомогательные методы в представлениях.</span><span class="sxs-lookup"><span data-stu-id="b1eac-195">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="b1eac-196">Вспомогательных функций тегов применяются как атрибуты для HTML-элементов, которые будут пропускаться редакторов, которые не знакомы с ними, позволяя просмотреть разметку на редактирование и подготовке к просмотру в различных средств.</span><span class="sxs-lookup"><span data-stu-id="b1eac-196">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="b1eac-197">Имеют множество применений вспомогательных функций тегов и в частности, можно сделать [работа с формами](working-with-forms.md) гораздо проще.</span><span class="sxs-lookup"><span data-stu-id="b1eac-197">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="b1eac-198">Создания настраиваемой HTML-разметки, может осуществляться в соответствии с многие встроенные вспомогательные методы HTML и более сложная логика пользовательского интерфейса (возможно, с собственными требованиями данных) можно инкапсулировать в [Просмотр компонентов](view-components.md).</span><span class="sxs-lookup"><span data-stu-id="b1eac-198">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="b1eac-199">Просмотр компонентов обеспечить же разделение областей ответственности, предлагающий контроллеры и представления и позволяет избавиться от необходимости для действий и представления для обработки данных, используемых в общих элементов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b1eac-199">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="b1eac-200">Как и многие другие аспекты ASP.NET Core представления поддерживают [внедрения зависимостей](../../fundamentals/dependency-injection.md), позволяя службам для [внедрены в представлениях](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="b1eac-200">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
