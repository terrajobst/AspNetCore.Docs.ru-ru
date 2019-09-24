---
title: Библиотеки классов компонентов Razor ASP.NET Core
author: guardrex
description: Узнайте, как компоненты можно включать в приложения Блазор из библиотеки внешних компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/class-libraries
ms.openlocfilehash: d9ef276357e95d97b7d89427c5e237aceea7a0d3
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207103"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="59ef5-103">Библиотеки классов компонентов Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59ef5-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="59ef5-104">По [Simon Тиммс](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="59ef5-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="59ef5-105">Компоненты можно совместно использовать в [библиотеке классов Razor (РКЛ)](xref:razor-pages/ui-class) в разных проектах.</span><span class="sxs-lookup"><span data-stu-id="59ef5-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="59ef5-106">*Библиотеку классов компонентов Razor* можно включать в:</span><span class="sxs-lookup"><span data-stu-id="59ef5-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="59ef5-107">Другой проект в решении.</span><span class="sxs-lookup"><span data-stu-id="59ef5-107">Another project in the solution.</span></span>
* <span data-ttu-id="59ef5-108">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="59ef5-108">A NuGet package.</span></span>
* <span data-ttu-id="59ef5-109">Упоминаемая библиотека .NET.</span><span class="sxs-lookup"><span data-stu-id="59ef5-109">A referenced .NET library.</span></span>

<span data-ttu-id="59ef5-110">Так же как и компоненты — обычные типы .NET, компоненты, предоставляемые РКЛ, являются нормальными сборками .NET.</span><span class="sxs-lookup"><span data-stu-id="59ef5-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="59ef5-111">Создание РКЛ</span><span class="sxs-lookup"><span data-stu-id="59ef5-111">Create an RCL</span></span>

<span data-ttu-id="59ef5-112">Следуйте указаниям, приведенным в <xref:blazor/get-started> статье, чтобы настроить среду для блазор.</span><span class="sxs-lookup"><span data-stu-id="59ef5-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59ef5-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59ef5-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="59ef5-114">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="59ef5-114">Create a new project.</span></span>
1. <span data-ttu-id="59ef5-115">Выберите пункт **Библиотека классов Razor**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="59ef5-116">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-116">Select **Next**.</span></span>
1. <span data-ttu-id="59ef5-117">В диалоговом окне **Создание новой библиотеки классов Razor** выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="59ef5-118">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="59ef5-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="59ef5-119">В примерах этого раздела используется имя `MyComponentLib1`проекта.</span><span class="sxs-lookup"><span data-stu-id="59ef5-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="59ef5-120">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-120">Select **Create**.</span></span>
1. <span data-ttu-id="59ef5-121">Добавьте РКЛ в решение:</span><span class="sxs-lookup"><span data-stu-id="59ef5-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="59ef5-122">Щелкните решение правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="59ef5-122">Right-click the solution.</span></span> <span data-ttu-id="59ef5-123">Выберите **Добавить** > **существующий проект**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="59ef5-124">Перейдите к файлу проекта РКЛ.</span><span class="sxs-lookup"><span data-stu-id="59ef5-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="59ef5-125">Выберите файл проекта РКЛ ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="59ef5-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="59ef5-126">Добавьте ссылку на РКЛ из приложения:</span><span class="sxs-lookup"><span data-stu-id="59ef5-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="59ef5-127">Щелкните проект приложения правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="59ef5-127">Right-click the app project.</span></span> <span data-ttu-id="59ef5-128">Выберите **Добавить** > **ссылку**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="59ef5-129">Выберите проект РКЛ.</span><span class="sxs-lookup"><span data-stu-id="59ef5-129">Select the RCL project.</span></span> <span data-ttu-id="59ef5-130">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="59ef5-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="59ef5-131">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="59ef5-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="59ef5-132">Используйте шаблон **библиотеки классов Razor** (`razorclasslib`) с командой [DotNet New](/dotnet/core/tools/dotnet-new) в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="59ef5-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="59ef5-133">В следующем примере создается РКЛ с именем `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="59ef5-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="59ef5-134">Папка, которая хранится `MyComponentLib1` , создается автоматически при выполнении команды:</span><span class="sxs-lookup"><span data-stu-id="59ef5-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="59ef5-135">Чтобы добавить библиотеку в существующий проект, используйте команду [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="59ef5-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="59ef5-136">В следующем примере РКЛ добавляется в приложение.</span><span class="sxs-lookup"><span data-stu-id="59ef5-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="59ef5-137">Выполните следующую команду из папки проекта приложения, указав путь к библиотеке:</span><span class="sxs-lookup"><span data-stu-id="59ef5-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="59ef5-138">Использование компонента библиотеки</span><span class="sxs-lookup"><span data-stu-id="59ef5-138">Consume a library component</span></span>

<span data-ttu-id="59ef5-139">Для использования компонентов, определенных в библиотеке в другом проекте, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="59ef5-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="59ef5-140">Используйте полное имя типа с пространством имен.</span><span class="sxs-lookup"><span data-stu-id="59ef5-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="59ef5-141">Используйте директиву [ \@using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="59ef5-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="59ef5-142">Отдельные компоненты можно добавлять по имени.</span><span class="sxs-lookup"><span data-stu-id="59ef5-142">Individual components can be added by name.</span></span>

<span data-ttu-id="59ef5-143">В следующих примерах — `MyComponentLib1` это библиотека компонентов, `SalesReport` содержащая компонент.</span><span class="sxs-lookup"><span data-stu-id="59ef5-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="59ef5-144">На `SalesReport` компонент можно ссылаться с помощью полного имени типа с пространством имен:</span><span class="sxs-lookup"><span data-stu-id="59ef5-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="59ef5-145">На компонент также можно ссылаться, если библиотека входит в область с `@using` директивой:</span><span class="sxs-lookup"><span data-stu-id="59ef5-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="59ef5-146">Включите директиву в файл *_Import. Razor* верхнего уровня, чтобы сделать компоненты библиотеки доступными для всего проекта. `@using MyComponentLib1`</span><span class="sxs-lookup"><span data-stu-id="59ef5-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="59ef5-147">Добавьте директиву в файл *_Import. Razor* на любом уровне, чтобы применить пространство имен к одной странице или набору страниц в папке.</span><span class="sxs-lookup"><span data-stu-id="59ef5-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="59ef5-148">Сборка, Упаковка и доставка в NuGet</span><span class="sxs-lookup"><span data-stu-id="59ef5-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="59ef5-149">Так как библиотеки компонентов являются стандартными библиотеками .NET, их упаковка и доставка в NuGet не отличается от упаковки и доставки любых библиотек в NuGet.</span><span class="sxs-lookup"><span data-stu-id="59ef5-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="59ef5-150">Упаковка выполняется с помощью команды [DotNet Pack](/dotnet/core/tools/dotnet-pack) в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="59ef5-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="59ef5-151">Отправьте пакет в NuGet с помощью команды [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="59ef5-151">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```dotnetcli
dotnet nuget publish
```

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="59ef5-152">Создание библиотеки классов компонентов Razor со статическими ресурсами</span><span class="sxs-lookup"><span data-stu-id="59ef5-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="59ef5-153">РКЛ может включать статические ресурсы.</span><span class="sxs-lookup"><span data-stu-id="59ef5-153">An RCL can include static assets.</span></span> <span data-ttu-id="59ef5-154">Статические ресурсы доступны для любого приложения, использующего библиотеку.</span><span class="sxs-lookup"><span data-stu-id="59ef5-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="59ef5-155">Дополнительные сведения см. в разделе <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="59ef5-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59ef5-156">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="59ef5-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
