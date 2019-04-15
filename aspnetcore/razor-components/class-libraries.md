---
title: Библиотеки классов Razor компонентов
author: guardrex
description: Узнайте, как компоненты могут быть включены в приложениях Razor компоненты из библиотеки внешнего компонента.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515590"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="1a409-103">Библиотеки классов Razor компонентов</span><span class="sxs-lookup"><span data-stu-id="1a409-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="1a409-104">По [Тиммса Simon](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="1a409-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="1a409-105">Компоненты могут использоваться совместно в библиотеках классов Razor проектов.</span><span class="sxs-lookup"><span data-stu-id="1a409-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="1a409-106">Компоненты можно включить из:</span><span class="sxs-lookup"><span data-stu-id="1a409-106">Components can be included from:</span></span>

* <span data-ttu-id="1a409-107">Другой проект в решении.</span><span class="sxs-lookup"><span data-stu-id="1a409-107">Another project in the solution.</span></span>
* <span data-ttu-id="1a409-108">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a409-108">A NuGet package.</span></span>
* <span data-ttu-id="1a409-109">Библиотека .NET, на которую указывает ссылка.</span><span class="sxs-lookup"><span data-stu-id="1a409-109">A referenced .NET library.</span></span>

<span data-ttu-id="1a409-110">Так же, как компоненты регулярные типы .NET, компоненты, предоставляемые в библиотеках классов Razor — это обычные сборки .NET.</span><span class="sxs-lookup"><span data-stu-id="1a409-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="1a409-111">Используйте `razorclasslib` шаблон (библиотека классов Razor) с [команды dotnet new](/dotnet/core/tools/dotnet-new) команды:</span><span class="sxs-lookup"><span data-stu-id="1a409-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="1a409-112">Добавить файлы Razor компонентов (*.razor*) для библиотеки классов Razor.</span><span class="sxs-lookup"><span data-stu-id="1a409-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="1a409-113">Чтобы добавить в библиотеку в существующий проект, используйте [dotnet sln](/dotnet/core/tools/dotnet-sln) команды:</span><span class="sxs-lookup"><span data-stu-id="1a409-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a409-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a409-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a409-115">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="1a409-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="1a409-116">Библиотеки классов Razor не совместимы с приложениями Blazor в ASP.NET Core предварительной версии 3.</span><span class="sxs-lookup"><span data-stu-id="1a409-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="1a409-117">Для создания компонентов библиотеки, который можно использовать совместно с Blazor и Razor компоненты приложения, используйте библиотеку классов Blazor, созданные `blazorlib` шаблона.</span><span class="sxs-lookup"><span data-stu-id="1a409-117">To create components in a library that can be shared with Blazor and Razor Components apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="1a409-118">Библиотеки классов Razor не поддерживают статических ресурсов в ASP.NET Core предварительной версии 3.</span><span class="sxs-lookup"><span data-stu-id="1a409-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="1a409-119">С помощью библиотек компонентов `blazorlib` шаблон может включать статические файлы, такие как изображения, JavaScript и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="1a409-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="1a409-120">Во время сборки, статические файлы внедряются в сборку файл (*.dll*), позволяющий потребления компонентов не нужно беспокоиться о том, как включить их ресурсы.</span><span class="sxs-lookup"><span data-stu-id="1a409-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="1a409-121">Все файлы, включенные в `content` directory помечаются как внедренный ресурс.</span><span class="sxs-lookup"><span data-stu-id="1a409-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="1a409-122">Использует компонент библиотеки</span><span class="sxs-lookup"><span data-stu-id="1a409-122">Consume a library component</span></span>

<span data-ttu-id="1a409-123">Чтобы использовать компоненты, определенные в библиотеке в другом проекте [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) необходимо использовать директиву.</span><span class="sxs-lookup"><span data-stu-id="1a409-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="1a409-124">Отдельные компоненты могут быть добавлены по имени.</span><span class="sxs-lookup"><span data-stu-id="1a409-124">Individual components may be added by name.</span></span>

<span data-ttu-id="1a409-125">Директивы общие выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1a409-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="1a409-126">Например, следующая директива добавляет `Component1` из `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="1a409-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="1a409-127">Тем не менее, довольно часто для включения всех компонентов из сборки с помощью подстановочного знака (`*`):</span><span class="sxs-lookup"><span data-stu-id="1a409-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="1a409-128">`@addTagHelper` Директивы может быть включено в *_ViewImport.cshtml* для этих компонентов доступны для всего проекта или примененные к одной странице или набору страниц в папке.</span><span class="sxs-lookup"><span data-stu-id="1a409-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="1a409-129">С помощью `@addTagHelper` директив на месте, компоненты библиотеки компонентов могут быть использованы как если бы они были в той же сборке, что и приложение.</span><span class="sxs-lookup"><span data-stu-id="1a409-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="1a409-130">Сборки, пакет и поставки для NuGet</span><span class="sxs-lookup"><span data-stu-id="1a409-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="1a409-131">Так как компонент библиотеки — стандартные библиотеки .NET, упаковки и их отправки в NuGet в качестве примеров, ничем не отличается от упаковки и доставки любую библиотеку в NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a409-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="1a409-132">Упаковка выполняется с помощью [dotnet pack](/dotnet/core/tools/dotnet-pack) команды:</span><span class="sxs-lookup"><span data-stu-id="1a409-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="1a409-133">Отправка пакета NuGet с помощью [публикации dotnet nuget](/dotnet/core/tools/dotnet-nuget-push) команды:</span><span class="sxs-lookup"><span data-stu-id="1a409-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="1a409-134">При использовании `blazorlib` шаблона, статические ресурсы будут включены в пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a409-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="1a409-135">Потребители библиотеки автоматически получать скриптов и таблиц стилей, чтобы потребители не требуется вручную установить ресурсы.</span><span class="sxs-lookup"><span data-stu-id="1a409-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
