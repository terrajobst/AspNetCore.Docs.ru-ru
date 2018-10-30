---
title: Введение в ASP.NET Core
author: rick-anderson
description: Введение в ASP.NET Core — кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: 37448b1b3d0da4e3cb34b1cd51f663b7e53ddced
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207398"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="4907f-103">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4907f-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="4907f-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="4907f-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="4907f-105">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="4907f-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="4907f-106">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="4907f-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="4907f-107">Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="4907f-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="4907f-108">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="4907f-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="4907f-109">Выполнять развертывания в облаке или локальной среде.</span><span class="sxs-lookup"><span data-stu-id="4907f-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="4907f-110">Работать в [.NET Core или .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="4907f-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="4907f-111">Зачем использовать ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="4907f-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="4907f-112">Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](/aspnet/overview) для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="4907f-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="4907f-113">ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="4907f-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="4907f-114">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="4907f-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="4907f-115">ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/index#build-web-apis) и [веб-приложения](xref:tutorials/index#build-web-apps).</span><span class="sxs-lookup"><span data-stu-id="4907f-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="4907f-116">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения [тестируемыми](xref:test/index).</span><span class="sxs-lookup"><span data-stu-id="4907f-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="4907f-117">[Страницы Razor](xref:razor-pages/index) (новый компонент в ASP.NET Core 2.0) — это основанная на страницах модель программирования, которая упрощает создание пользовательского веб-интерфейса и повышает его эффективность.</span><span class="sxs-lookup"><span data-stu-id="4907f-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="4907f-118">[Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="4907f-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="4907f-119">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="4907f-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="4907f-120">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="4907f-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="4907f-121">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.</span><span class="sxs-lookup"><span data-stu-id="4907f-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="4907f-122">[Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="4907f-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="4907f-123">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="4907f-123">Client-side development</span></span>

<span data-ttu-id="4907f-124">ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="4907f-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="4907f-125">Дополнительные сведения см. в разделе о [клиентской разработке](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="4907f-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="4907f-126">ASP.NET Core для платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4907f-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="4907f-127">Приложения ASP.NET Core могут выполняться в .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4907f-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="4907f-128">Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows.</span><span class="sxs-lookup"><span data-stu-id="4907f-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="4907f-129">Отказ от поддержки .NET Framework в ASP.NET Core не планируется.</span><span class="sxs-lookup"><span data-stu-id="4907f-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="4907f-130">Как правило, ASP.NET Core состоит из библиотек [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="4907f-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="4907f-131">Приложения, написанные для .NET Standard 2.0 запускаются везде, где есть поддержка .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="4907f-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="4907f-132">ASP.NET Core 2.x поддерживается в версиях .NET Framework, совместимых с .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="4907f-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="4907f-133">.NET Framework 4.7.1 и более поздних версий (рекомендуется).</span><span class="sxs-lookup"><span data-stu-id="4907f-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="4907f-134">.NET Framework 4.6.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="4907f-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="4907f-135">При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском.</span><span class="sxs-lookup"><span data-stu-id="4907f-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="4907f-136">Преимущества .NET Core по сравнению с .NET Framework включают:</span><span class="sxs-lookup"><span data-stu-id="4907f-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="4907f-137">Кроссплатформенность.</span><span class="sxs-lookup"><span data-stu-id="4907f-137">Cross-platform.</span></span> <span data-ttu-id="4907f-138">Выполняется на macOS, Linux и Windows.</span><span class="sxs-lookup"><span data-stu-id="4907f-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="4907f-139">Повышение производительности</span><span class="sxs-lookup"><span data-stu-id="4907f-139">Improved performance</span></span>
* <span data-ttu-id="4907f-140">Управление параллельными версиями</span><span class="sxs-lookup"><span data-stu-id="4907f-140">Side-by-side versioning</span></span>
* <span data-ttu-id="4907f-141">Новые интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="4907f-141">New APIs</span></span>
* <span data-ttu-id="4907f-142">Открытый исходный код</span><span class="sxs-lookup"><span data-stu-id="4907f-142">Open source</span></span>

<span data-ttu-id="4907f-143">Мы прилагаем максимум усилий, чтобы устранить различия API между .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4907f-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="4907f-144">Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows.</span><span class="sxs-lookup"><span data-stu-id="4907f-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="4907f-145">Эти API-интерфейсы не были доступны в .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4907f-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="4907f-146">Загрузка примера</span><span class="sxs-lookup"><span data-stu-id="4907f-146">How to download a sample</span></span>

<span data-ttu-id="4907f-147">Многие статьи и учебники содержат ссылки на примеры кода.</span><span class="sxs-lookup"><span data-stu-id="4907f-147">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="4907f-148">[Загрузите ZIP-файл репозитория ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="4907f-148">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="4907f-149">Распакуйте файл *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="4907f-149">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="4907f-150">Перейдите в папку примера по URL-адресу, указанному в примере.</span><span class="sxs-lookup"><span data-stu-id="4907f-150">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4907f-151">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4907f-151">Next steps</span></span>

<span data-ttu-id="4907f-152">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="4907f-152">For more information, see the following resources:</span></span>

* [<span data-ttu-id="4907f-153">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4907f-153">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="4907f-154">Учебники по ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4907f-154">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="4907f-155">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4907f-155">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="4907f-156">[В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды.</span><span class="sxs-lookup"><span data-stu-id="4907f-156">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="4907f-157">Помимо этого, публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="4907f-157">It features new blogs and third-party software.</span></span>
