---
title: Введение в ASP.NET Core
author: rick-anderson
description: Введение в ASP.NET Core — кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: 3e41b329feb359c006258b8ed558496dfcbbf3b0
ms.sourcegitcommit: 57b85708f4cded99b8f008a69830cb104cd8e879
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2020
ms.locfileid: "75914222"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="a766a-103">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a766a-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="a766a-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="a766a-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="a766a-105">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="a766a-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="a766a-106">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="a766a-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="a766a-107">Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="a766a-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="a766a-108">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="a766a-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="a766a-109">Выполнять развертывания в облаке или локальной среде.</span><span class="sxs-lookup"><span data-stu-id="a766a-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="a766a-110">Работать в [.NET Core или .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="a766a-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="a766a-111">Преимущества, обеспечиваемые ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a766a-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="a766a-112">Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](/aspnet/overview) для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="a766a-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="a766a-113">ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="a766a-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="a766a-114">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a766a-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="a766a-115">ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/first-web-api) и [веб-приложения](xref:tutorials/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="a766a-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="a766a-116">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения тестируемыми.</span><span class="sxs-lookup"><span data-stu-id="a766a-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="a766a-117">[Razor Pages](xref:razor-pages/index) — это основанная на страницах модель программирования, которая упрощает и повышает эффективность создания пользовательского веб-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a766a-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="a766a-118">[Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="a766a-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="a766a-119">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="a766a-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="a766a-120">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="a766a-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="a766a-121">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.</span><span class="sxs-lookup"><span data-stu-id="a766a-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="a766a-122">[Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="a766a-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="a766a-123">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="a766a-123">Client-side development</span></span>

<span data-ttu-id="a766a-124">ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="a766a-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="a766a-125">Подробнее см. <xref:blazor/index> и связанные материалы о *разработке на стороне клиента*.</span><span class="sxs-lookup"><span data-stu-id="a766a-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="a766a-126">ASP.NET Core для платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a766a-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="a766a-127">Приложения ASP.NET Core 2.x могут выполняться в .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a766a-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="a766a-128">Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows.</span><span class="sxs-lookup"><span data-stu-id="a766a-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="a766a-129">Как правило, ASP.NET Core 2.x состоит из библиотек [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="a766a-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="a766a-130">Библиотеки, написанные на .NET Standard 2.0 под управлением любой [платформы .NET с реализацией .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="a766a-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="a766a-131">ASP.NET Core 2.x поддерживается в версиях .NET Framework с реализацией .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="a766a-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="a766a-132">Настоятельно рекомендуем использовать последнюю версию .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a766a-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="a766a-133">.NET Framework 4.6.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="a766a-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="a766a-134">ASP.NET Core версии 3.0 и более поздних будут выполняться только в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a766a-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="a766a-135">Дополнительные сведения об этом изменении см. в разделе [Первое знакомство с предстоящими изменениями в ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="a766a-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="a766a-136">При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском.</span><span class="sxs-lookup"><span data-stu-id="a766a-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="a766a-137">Преимущества .NET Core по сравнению с .NET Framework включают:</span><span class="sxs-lookup"><span data-stu-id="a766a-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="a766a-138">Кроссплатформенность.</span><span class="sxs-lookup"><span data-stu-id="a766a-138">Cross-platform.</span></span> <span data-ttu-id="a766a-139">Выполняется на macOS, Linux и Windows.</span><span class="sxs-lookup"><span data-stu-id="a766a-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="a766a-140">Повышение производительности</span><span class="sxs-lookup"><span data-stu-id="a766a-140">Improved performance</span></span>
* [<span data-ttu-id="a766a-141">Управление параллельными версиями</span><span class="sxs-lookup"><span data-stu-id="a766a-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="a766a-142">Новые интерфейсы API</span><span class="sxs-lookup"><span data-stu-id="a766a-142">New APIs</span></span>
* <span data-ttu-id="a766a-143">Открытый исходный код</span><span class="sxs-lookup"><span data-stu-id="a766a-143">Open source</span></span>

<span data-ttu-id="a766a-144">Мы прилагаем максимум усилий, чтобы устранить различия API между .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a766a-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="a766a-145">Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows.</span><span class="sxs-lookup"><span data-stu-id="a766a-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="a766a-146">Эти API-интерфейсы не были доступны в .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a766a-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="a766a-147">Рекомендуемая схема обучения</span><span class="sxs-lookup"><span data-stu-id="a766a-147">Recommended learning path</span></span>

<span data-ttu-id="a766a-148">Для знакомства с разработкой приложений ASP.NET Core рекомендуется изучить следующую последовательность учебников и статей.</span><span class="sxs-lookup"><span data-stu-id="a766a-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="a766a-149">Пройдите учебник по тому типу приложения, которое вы собираетесь разрабатывать или обслуживать:</span><span class="sxs-lookup"><span data-stu-id="a766a-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="a766a-150">Тип приложения</span><span class="sxs-lookup"><span data-stu-id="a766a-150">App type</span></span>  |<span data-ttu-id="a766a-151">Сценарий</span><span class="sxs-lookup"><span data-stu-id="a766a-151">Scenario</span></span>  |<span data-ttu-id="a766a-152">Учебник</span><span class="sxs-lookup"><span data-stu-id="a766a-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="a766a-153">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="a766a-153">Web app</span></span>                   | <span data-ttu-id="a766a-154">Разработка нового приложения</span><span class="sxs-lookup"><span data-stu-id="a766a-154">For new development</span></span>        |[<span data-ttu-id="a766a-155">Начало работы с Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a766a-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="a766a-156">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="a766a-156">Web app</span></span>                   | <span data-ttu-id="a766a-157">Обслуживание приложения MVC</span><span class="sxs-lookup"><span data-stu-id="a766a-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="a766a-158">Начало работы с MVC</span><span class="sxs-lookup"><span data-stu-id="a766a-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="a766a-159">Веб-интерфейс API</span><span class="sxs-lookup"><span data-stu-id="a766a-159">Web API</span></span>                   |                            |<span data-ttu-id="a766a-160">[Создание веб-API](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="a766a-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="a766a-161">Приложение режима реального времени</span><span class="sxs-lookup"><span data-stu-id="a766a-161">Real-time app</span></span>             |                            |<span data-ttu-id="a766a-162">[Начало работы с SignalR](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="a766a-162">[Get started with SignalR](xref:tutorials/signalr)</span></span> |
   |<span data-ttu-id="a766a-163">Приложение Blazor</span><span class="sxs-lookup"><span data-stu-id="a766a-163">Blazor app</span></span>                |                            |<span data-ttu-id="a766a-164">[Начало работы с Blazor](xref:blazor/get-started)</span><span class="sxs-lookup"><span data-stu-id="a766a-164">[Get started with Blazor](xref:blazor/get-started)</span></span> |
   |<span data-ttu-id="a766a-165">Приложение удаленного вызова процедур</span><span class="sxs-lookup"><span data-stu-id="a766a-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="a766a-166">Начало работы со службой gRPC</span><span class="sxs-lookup"><span data-stu-id="a766a-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="a766a-167">Пройдите учебник, посвященный основам доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="a766a-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="a766a-168">Сценарий</span><span class="sxs-lookup"><span data-stu-id="a766a-168">Scenario</span></span>  |<span data-ttu-id="a766a-169">Учебник</span><span class="sxs-lookup"><span data-stu-id="a766a-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="a766a-170">Разработка нового приложения</span><span class="sxs-lookup"><span data-stu-id="a766a-170">For new development</span></span>        |[<span data-ttu-id="a766a-171">Razor Pages с Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a766a-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="a766a-172">Обслуживание приложения MVC</span><span class="sxs-lookup"><span data-stu-id="a766a-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="a766a-173">MVC с Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a766a-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="a766a-174">Прочтите обзор функций ASP.NET Core, относящихся ко всем типам приложений:</span><span class="sxs-lookup"><span data-stu-id="a766a-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="a766a-175">Основы</span><span class="sxs-lookup"><span data-stu-id="a766a-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="a766a-176">Просмотрите содержание, чтобы найти другие интересующие вас темы.</span><span class="sxs-lookup"><span data-stu-id="a766a-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="a766a-177">\* Доступен новый [учебник по веб-API с прохождением в браузере](https://docs.microsoft.com/learn/modules/build-web-api-net-core), не требующий установки локальной интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="a766a-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="a766a-178">Код выполняется в [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), а для тестирования используется [curl](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="a766a-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="a766a-179">Миграция с .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a766a-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="a766a-180">Справочное руководство по миграции приложений ASP.NET на ASP.NET Core см. в статье <xref:migration/proper-to-2x/index>.</span><span class="sxs-lookup"><span data-stu-id="a766a-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="a766a-181">Загрузка примера</span><span class="sxs-lookup"><span data-stu-id="a766a-181">How to download a sample</span></span>

<span data-ttu-id="a766a-182">Многие статьи и учебники содержат ссылки на примеры кода.</span><span class="sxs-lookup"><span data-stu-id="a766a-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="a766a-183">[Загрузите ZIP-файл репозитория ASP.NET](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="a766a-183">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="a766a-184">Распакуйте файл *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="a766a-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="a766a-185">Перейдите в папку примера по URL-адресу, указанному в примере.</span><span class="sxs-lookup"><span data-stu-id="a766a-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="a766a-186">Директивы препроцессора в примере кода</span><span class="sxs-lookup"><span data-stu-id="a766a-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="a766a-187">Для демонстрации нескольких сценариев в примерах приложений используются директивы препроцессора `#define` и `#if-#else/#elif-#endif`, выборочно компилирующие и запускающие разные фрагменты примеров кода.</span><span class="sxs-lookup"><span data-stu-id="a766a-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="a766a-188">В примерах, где применяется этот подход, задайте в начале файлов C# директиву `#define` для определения символа, связанного со сценарием, который нужно запустить.</span><span class="sxs-lookup"><span data-stu-id="a766a-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="a766a-189">Для запуска сценария в некоторых примерах потребуется определить символ в начале нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="a766a-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="a766a-190">Например, в следующем списке символов `#define` видно, что доступно четыре сценария (один сценарий на символ).</span><span class="sxs-lookup"><span data-stu-id="a766a-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="a766a-191">В текущем примере конфигурации запускается сценарий `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="a766a-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="a766a-192">Чтобы запустить в примере сценарий `ExpandDefault`, задайте символ `ExpandDefault` и оставьте остальные символы раскомментированными:</span><span class="sxs-lookup"><span data-stu-id="a766a-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="a766a-193">Дополнительные сведения об использовании [директив препроцессора C#](/dotnet/csharp/language-reference/preprocessor-directives/) для выборочной компиляции фрагментов кода см. в разделах [#define (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) и [#if (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="a766a-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="a766a-194">Регионы в примере кода</span><span class="sxs-lookup"><span data-stu-id="a766a-194">Regions in sample code</span></span>

<span data-ttu-id="a766a-195">Некоторые примеры приложений содержат фрагменты кода внутри директив C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) и [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion).</span><span class="sxs-lookup"><span data-stu-id="a766a-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="a766a-196">Система сборки документации вставляет эти регионы в обработанные разделы документации.</span><span class="sxs-lookup"><span data-stu-id="a766a-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="a766a-197">Названия регионов обычно содержат слово "фрагмент".</span><span class="sxs-lookup"><span data-stu-id="a766a-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="a766a-198">В следующем примере показан регион с именем `snippet_WebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="a766a-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="a766a-199">На предыдущий фрагмент кода C# указывает ссылка в следующей строке в файле Markdown раздела:</span><span class="sxs-lookup"><span data-stu-id="a766a-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="a766a-200">Вы можете спокойно проигнорировать или удалить директивы `#region` и `#endregion` вокруг кода.</span><span class="sxs-lookup"><span data-stu-id="a766a-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="a766a-201">Не изменяйте код внутри этих директив, если планируете запустить примеры сценариев, описанные в разделе.</span><span class="sxs-lookup"><span data-stu-id="a766a-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="a766a-202">Вы можете изменить код, экспериментируя с другими сценариями.</span><span class="sxs-lookup"><span data-stu-id="a766a-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="a766a-203">Дополнительные сведения см. в разделе[Участие в написании документации ASP.NET: Фрагменты кода](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="a766a-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a766a-204">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a766a-204">Next steps</span></span>

<span data-ttu-id="a766a-205">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="a766a-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="a766a-206">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a766a-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="a766a-207">[В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды.</span><span class="sxs-lookup"><span data-stu-id="a766a-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="a766a-208">Помимо этого, публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="a766a-208">It features new blogs and third-party software.</span></span>
