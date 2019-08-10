---
title: Библиотеки классов компонентов Razor ASP.NET Core
author: guardrex
description: Узнайте, как компоненты можно включать в приложения Блазор из библиотеки внешних компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948444"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="205b2-103">Библиотеки классов компонентов Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="205b2-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="205b2-104">По [Simon Тиммс](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="205b2-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="205b2-105">Компоненты можно совместно использовать в [библиотеке классов Razor (РКЛ)](xref:razor-pages/ui-class) в разных проектах.</span><span class="sxs-lookup"><span data-stu-id="205b2-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="205b2-106">*Библиотеку классов компонентов Razor* можно включать в:</span><span class="sxs-lookup"><span data-stu-id="205b2-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="205b2-107">Другой проект в решении.</span><span class="sxs-lookup"><span data-stu-id="205b2-107">Another project in the solution.</span></span>
* <span data-ttu-id="205b2-108">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="205b2-108">A NuGet package.</span></span>
* <span data-ttu-id="205b2-109">Упоминаемая библиотека .NET.</span><span class="sxs-lookup"><span data-stu-id="205b2-109">A referenced .NET library.</span></span>

<span data-ttu-id="205b2-110">Так же как и компоненты — обычные типы .NET, компоненты, предоставляемые РКЛ, являются нормальными сборками .NET.</span><span class="sxs-lookup"><span data-stu-id="205b2-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="205b2-111">Создание РКЛ</span><span class="sxs-lookup"><span data-stu-id="205b2-111">Create an RCL</span></span>

<span data-ttu-id="205b2-112">Следуйте указаниям, приведенным в <xref:blazor/get-started> статье, чтобы настроить среду для блазор.</span><span class="sxs-lookup"><span data-stu-id="205b2-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="205b2-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="205b2-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="205b2-114">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="205b2-114">Create a new project.</span></span>
1. <span data-ttu-id="205b2-115">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="205b2-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="205b2-116">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="205b2-116">Select **Next**.</span></span>
1. <span data-ttu-id="205b2-117">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="205b2-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="205b2-118">В примерах этого раздела используется имя `MyComponentLib1`проекта.</span><span class="sxs-lookup"><span data-stu-id="205b2-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="205b2-119">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="205b2-119">Select **Create**.</span></span>
1. <span data-ttu-id="205b2-120">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="205b2-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="205b2-121">Выберите шаблон " **Библиотека классов Razor** ".</span><span class="sxs-lookup"><span data-stu-id="205b2-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="205b2-122">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="205b2-122">Select **Create**.</span></span>
1. <span data-ttu-id="205b2-123">Добавьте РКЛ в решение:</span><span class="sxs-lookup"><span data-stu-id="205b2-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="205b2-124">Щелкните решение правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="205b2-124">Right-click the solution.</span></span> <span data-ttu-id="205b2-125">Выберите **Добавить** > **существующий проект**.</span><span class="sxs-lookup"><span data-stu-id="205b2-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="205b2-126">Перейдите к файлу проекта РКЛ.</span><span class="sxs-lookup"><span data-stu-id="205b2-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="205b2-127">Выберите файл проекта РКЛ ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="205b2-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="205b2-128">Добавьте ссылку на РКЛ из приложения:</span><span class="sxs-lookup"><span data-stu-id="205b2-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="205b2-129">Щелкните проект приложения правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="205b2-129">Right-click the app project.</span></span> <span data-ttu-id="205b2-130">Выберите **Добавить** > **ссылку**.</span><span class="sxs-lookup"><span data-stu-id="205b2-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="205b2-131">Выберите проект РКЛ.</span><span class="sxs-lookup"><span data-stu-id="205b2-131">Select the RCL project.</span></span> <span data-ttu-id="205b2-132">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="205b2-132">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="205b2-133">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="205b2-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="205b2-134">Используйте шаблон **библиотеки классов Razor** (`razorclasslib`) с командой [DotNet New](/dotnet/core/tools/dotnet-new) в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="205b2-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="205b2-135">В следующем примере создается РКЛ с именем `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="205b2-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="205b2-136">Папка, которая хранится `MyComponentLib1` , создается автоматически при выполнении команды:</span><span class="sxs-lookup"><span data-stu-id="205b2-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="205b2-137">Чтобы добавить библиотеку в существующий проект, используйте команду [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="205b2-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="205b2-138">В следующем примере РКЛ добавляется в приложение.</span><span class="sxs-lookup"><span data-stu-id="205b2-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="205b2-139">Выполните следующую команду из папки проекта приложения, указав путь к библиотеке:</span><span class="sxs-lookup"><span data-stu-id="205b2-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="205b2-140">Рклс не поддерживается для клиентских приложений</span><span class="sxs-lookup"><span data-stu-id="205b2-140">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="205b2-141">В текущей предварительной версии ASP.NET Core 3,0 библиотеки классов Razor несовместимы с клиентскими приложениями Блазор.</span><span class="sxs-lookup"><span data-stu-id="205b2-141">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="205b2-142">Для клиентских приложений блазор используйте библиотеку компонентов блазор, созданную с `blazorlib` помощью шаблона, в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="205b2-142">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="205b2-143">Библиотеки компонентов, использующие `blazorlib` шаблон, могут включать статические файлы, такие как изображения, JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="205b2-143">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="205b2-144">Во время сборки статические файлы внедряются в файл сборки ( *. dll*), который позволяет использовать компоненты без необходимости беспокоиться о том, как включать их ресурсы.</span><span class="sxs-lookup"><span data-stu-id="205b2-144">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="205b2-145">Все файлы, `content` включенные в каталог, помечаются как внедренные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="205b2-145">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="205b2-146">Использование компонента библиотеки</span><span class="sxs-lookup"><span data-stu-id="205b2-146">Consume a library component</span></span>

<span data-ttu-id="205b2-147">Для использования компонентов, определенных в библиотеке в другом проекте, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="205b2-147">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="205b2-148">Используйте полное имя типа с пространством имен.</span><span class="sxs-lookup"><span data-stu-id="205b2-148">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="205b2-149">Используйте директиву [ \@using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="205b2-149">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="205b2-150">Отдельные компоненты можно добавлять по имени.</span><span class="sxs-lookup"><span data-stu-id="205b2-150">Individual components can be added by name.</span></span>

<span data-ttu-id="205b2-151">В следующих примерах — `MyComponentLib1` это библиотека компонентов, `SalesReport` содержащая компонент.</span><span class="sxs-lookup"><span data-stu-id="205b2-151">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="205b2-152">На `SalesReport` компонент можно ссылаться с помощью полного имени типа с пространством имен:</span><span class="sxs-lookup"><span data-stu-id="205b2-152">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="205b2-153">На компонент также можно ссылаться, если библиотека входит в область с `@using` директивой:</span><span class="sxs-lookup"><span data-stu-id="205b2-153">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="205b2-154">Включите директиву в файл *_Import. Razor* верхнего уровня, чтобы сделать компоненты библиотеки доступными для всего проекта. `@using MyComponentLib1`</span><span class="sxs-lookup"><span data-stu-id="205b2-154">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="205b2-155">Добавьте директиву в файл *_Import. Razor* на любом уровне, чтобы применить пространство имен к одной странице или набору страниц в папке.</span><span class="sxs-lookup"><span data-stu-id="205b2-155">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="205b2-156">Сборка, Упаковка и доставка в NuGet</span><span class="sxs-lookup"><span data-stu-id="205b2-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="205b2-157">Так как библиотеки компонентов являются стандартными библиотеками .NET, их упаковка и доставка в NuGet не отличается от упаковки и доставки любых библиотек в NuGet.</span><span class="sxs-lookup"><span data-stu-id="205b2-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="205b2-158">Упаковка выполняется с помощью команды [DotNet Pack](/dotnet/core/tools/dotnet-pack) в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="205b2-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="205b2-159">Отправьте пакет в NuGet с помощью команды [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) в командной оболочке:</span><span class="sxs-lookup"><span data-stu-id="205b2-159">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="205b2-160">При использовании `blazorlib` шаблона статические ресурсы включаются в пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="205b2-160">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="205b2-161">Потребители библиотек автоматически получают скрипты и таблицы стилей, поэтому пользователям не требуется вручную устанавливать ресурсы.</span><span class="sxs-lookup"><span data-stu-id="205b2-161">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="205b2-162">Создание библиотеки классов компонентов Razor со статическими ресурсами</span><span class="sxs-lookup"><span data-stu-id="205b2-162">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="205b2-163">РКЛ может включать статические ресурсы.</span><span class="sxs-lookup"><span data-stu-id="205b2-163">An RCL can include static assets.</span></span> <span data-ttu-id="205b2-164">Статические ресурсы доступны для любого приложения, использующего библиотеку.</span><span class="sxs-lookup"><span data-stu-id="205b2-164">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="205b2-165">Дополнительные сведения см. в разделе <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="205b2-165">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="205b2-166">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="205b2-166">Additional resources</span></span>

* <xref:razor-pages/ui-class>
