---
title: "Введение в ASP.NET Core"
author: rick-anderson
description: "Общие сведения о работе с ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="17677-104">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17677-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="17677-105">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="17677-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="17677-106">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="17677-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="17677-107">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="17677-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="17677-108">Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/en-us/internet-of-things/) и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="17677-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="17677-109">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="17677-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="17677-110">Выполнять развертывания в облаке и локальной среде.</span><span class="sxs-lookup"><span data-stu-id="17677-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="17677-111">Работать в [.NET Core или .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="17677-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="17677-112">Зачем использовать ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="17677-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="17677-113">Миллионы разработчиков использовали и продолжают использовать ASP.NET для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="17677-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="17677-114">ASP.NET Core является модификацией ASP.NET с внесенными архитектурными изменениями, формирующими более рациональную и модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="17677-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="17677-115">ASP.NET Core предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="17677-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="17677-116">Единое решение для создания пользовательского веб-интерфейса и веб-API.</span><span class="sxs-lookup"><span data-stu-id="17677-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="17677-117">Интеграция [современных клиентских платформ](xref:client-side/index) и рабочих процессов разработки.</span><span class="sxs-lookup"><span data-stu-id="17677-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="17677-118">Облачная [система конфигурации](xref:fundamentals/configuration/index) на основе среды.</span><span class="sxs-lookup"><span data-stu-id="17677-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="17677-119">Встроенное [введение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="17677-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="17677-120">Упрощенный, высокопроизводительный и модульный конвейер HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="17677-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="17677-121">Возможность размещения в IIS или в собственном процессе.</span><span class="sxs-lookup"><span data-stu-id="17677-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="17677-122">Возможность запуска на платформе [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), которая поддерживает параллельное управление версиями приложения.</span><span class="sxs-lookup"><span data-stu-id="17677-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="17677-123">Инструментарий, упрощающий процесс современной веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="17677-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="17677-124">Возможность сборки и запуска в ОС Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="17677-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="17677-125">Открытый исходный код и ориентация на сообщество.</span><span class="sxs-lookup"><span data-stu-id="17677-125">Open-source and community-focused.</span></span>

<span data-ttu-id="17677-126">ASP.NET Core поставляется полностью в виде пакетов [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="17677-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="17677-127">Это позволяет оптимизировать приложения для включения только необходимых пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="17677-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="17677-128">За счет небольшого размера контактной зоны приложения доступны такие преимущества, как более высокий уровень безопасности, минимальное обслуживание и улучшенная производительность.</span><span class="sxs-lookup"><span data-stu-id="17677-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="17677-129">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="17677-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="17677-130">ASP.NET Core MVC предоставляет функциональные возможности, упрощающие создание [веб-API](xref:tutorials/index#building-web-apis) и [веб-приложений](xref:tutorials/index#building-web-applications).</span><span class="sxs-lookup"><span data-stu-id="17677-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="17677-131">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения [тестируемыми](testing/index.md).</span><span class="sxs-lookup"><span data-stu-id="17677-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="17677-132">[Страницы Razor](xref:mvc/razor-pages/index) (новинка в версии 2.0) — это основанная на страницах модель программирования, которая упрощает и повышает эффективность создания пользовательского веб-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="17677-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="17677-133">[Синтаксис Razor](xref:mvc/views/razor) предоставляет эффективный язык для [страниц Razor](xref:mvc/razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="17677-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="17677-134">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="17677-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="17677-135">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](mvc/models/formatting.md) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="17677-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="17677-136">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросы с параметрами метода действия.</span><span class="sxs-lookup"><span data-stu-id="17677-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="17677-137">[Проверка модели](xref:mvc/models/validation) автоматически выполняет проверку на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="17677-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="17677-138">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="17677-138">Client-side development</span></span>

<span data-ttu-id="17677-139">ASP.NET Core легко интегрируется с широким спектром клиентских платформ, включая [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) и [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="17677-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="17677-140">Дополнительные сведения см. в разделе о [клиентской разработке](client-side/index.md).</span><span class="sxs-lookup"><span data-stu-id="17677-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17677-141">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="17677-141">Next steps</span></span>

<span data-ttu-id="17677-142">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="17677-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="17677-143">Учебники по ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17677-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="17677-144">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17677-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="17677-145">[В еженедельном выпуске ASP.NET community standup](https://live.asp.net/) рассматривается ход работы и планы команды, а также публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="17677-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
