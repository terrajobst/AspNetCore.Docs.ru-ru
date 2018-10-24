---
title: Введение в ASP.NET Core
author: rick-anderson
description: Введение в ASP.NET Core — кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 3bb86fa255548ff66592ac14c1020e0c6b47959c
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391161"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="e245d-103">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e245d-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="e245d-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="e245d-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="e245d-105">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="e245d-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="e245d-106">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="e245d-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="e245d-107">Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="e245d-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="e245d-108">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="e245d-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e245d-109">Выполнять развертывания в облаке или локальной среде.</span><span class="sxs-lookup"><span data-stu-id="e245d-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="e245d-110">Работать в [.NET Core или .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e245d-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="e245d-111">Зачем использовать ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="e245d-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="e245d-112">Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="e245d-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="e245d-113">ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="e245d-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="e245d-114">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e245d-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="e245d-115">ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/index#build-web-apis) и [веб-приложения](xref:tutorials/index#build-web-apps).</span><span class="sxs-lookup"><span data-stu-id="e245d-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="e245d-116">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения [тестируемыми](xref:test/index).</span><span class="sxs-lookup"><span data-stu-id="e245d-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="e245d-117">[Страницы Razor](xref:razor-pages/index) (новый компонент в ASP.NET Core 2.0) — это основанная на страницах модель программирования, которая упрощает создание пользовательского веб-интерфейса и повышает его эффективность.</span><span class="sxs-lookup"><span data-stu-id="e245d-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="e245d-118">[Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="e245d-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="e245d-119">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="e245d-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="e245d-120">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="e245d-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="e245d-121">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.</span><span class="sxs-lookup"><span data-stu-id="e245d-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="e245d-122">[Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="e245d-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="e245d-123">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="e245d-123">Client-side development</span></span>

<span data-ttu-id="e245d-124">ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="e245d-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="e245d-125">Дополнительные сведения см. в разделе о [клиентской разработке](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="e245d-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="e245d-126">ASP.NET Core для платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="e245d-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="e245d-127">Приложения ASP.NET Core могут выполняться в .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e245d-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="e245d-128">Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows.</span><span class="sxs-lookup"><span data-stu-id="e245d-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="e245d-129">Отказ от поддержки .NET Framework в ASP.NET Core не планируется.</span><span class="sxs-lookup"><span data-stu-id="e245d-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="e245d-130">Как правило, ASP.NET Core состоит из библиотек [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="e245d-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="e245d-131">Приложения, написанные для .NET Standard 2.0 запускаются везде, где есть поддержка .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="e245d-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="e245d-132">ASP.NET Core 2.x поддерживается в версиях .NET Framework, совместимых с .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="e245d-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="e245d-133">.NET Framework 4.7.1 и более поздних версий (рекомендуется).</span><span class="sxs-lookup"><span data-stu-id="e245d-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="e245d-134">.NET Framework 4.6.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="e245d-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="e245d-135">При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском.</span><span class="sxs-lookup"><span data-stu-id="e245d-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="e245d-136">Преимущества .NET Core по сравнению с .NET Framework включают:</span><span class="sxs-lookup"><span data-stu-id="e245d-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="e245d-137">Кроссплатформенность.</span><span class="sxs-lookup"><span data-stu-id="e245d-137">Cross-platform.</span></span> <span data-ttu-id="e245d-138">Выполняется на macOS, Linux и Windows.</span><span class="sxs-lookup"><span data-stu-id="e245d-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="e245d-139">Повышение производительности</span><span class="sxs-lookup"><span data-stu-id="e245d-139">Improved performance</span></span>
* <span data-ttu-id="e245d-140">Управление параллельными версиями</span><span class="sxs-lookup"><span data-stu-id="e245d-140">Side-by-side versioning</span></span>
* <span data-ttu-id="e245d-141">Новые интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="e245d-141">New APIs</span></span>
* <span data-ttu-id="e245d-142">Открытый исходный код</span><span class="sxs-lookup"><span data-stu-id="e245d-142">Open source</span></span>

<span data-ttu-id="e245d-143">Мы прилагаем максимум усилий, чтобы устранить различия API между .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e245d-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="e245d-144">Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows.</span><span class="sxs-lookup"><span data-stu-id="e245d-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="e245d-145">Эти API-интерфейсы не были доступны в .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e245d-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e245d-146">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e245d-146">Next steps</span></span>

<span data-ttu-id="e245d-147">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="e245d-147">For more information, see the following resources:</span></span>

* [<span data-ttu-id="e245d-148">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e245d-148">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="e245d-149">Учебники по ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e245d-149">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="e245d-150">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e245d-150">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="e245d-151">[В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды.</span><span class="sxs-lookup"><span data-stu-id="e245d-151">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="e245d-152">Помимо этого, публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="e245d-152">It features new blogs and third-party software.</span></span>
