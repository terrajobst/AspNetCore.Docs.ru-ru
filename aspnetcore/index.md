---
title: "Введение в ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: d8516643393cdf9175adc78ab98420dc1dc5d1d9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="66a7f-103">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66a7f-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="66a7f-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="66a7f-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="66a7f-105">ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету.</span><span class="sxs-lookup"><span data-stu-id="66a7f-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="66a7f-106">ASP.NET Core позволяет выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="66a7f-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="66a7f-107">Создавать веб-приложения и службы, приложения IoT и серверные части для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="66a7f-107">Build web apps and services, IoT apps, and mobile backends.</span></span>
* <span data-ttu-id="66a7f-108">Использовать избранные средства разработки в Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="66a7f-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="66a7f-109">Выполнять развертывания в облаке и локальной среде.</span><span class="sxs-lookup"><span data-stu-id="66a7f-109">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="66a7f-110">Работать в [.NET Core или .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="66a7f-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="66a7f-111">Зачем использовать ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="66a7f-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="66a7f-112">Миллионы разработчиков использовали ASP.NET (и продолжают использовать) для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="66a7f-112">Millions of developers have used ASP.NET (and continue to use it) to create web apps.</span></span> <span data-ttu-id="66a7f-113">ASP.NET Core является модификацией ASP.NET с внесенными архитектурными изменениями, формирующими более рациональную и модульную платформу.</span><span class="sxs-lookup"><span data-stu-id="66a7f-113">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="66a7f-114">ASP.NET Core предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="66a7f-114">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="66a7f-115">Единое решение для создания пользовательского веб-интерфейса и веб-API.</span><span class="sxs-lookup"><span data-stu-id="66a7f-115">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="66a7f-116">Интеграция [современных клиентских платформ](xref:client-side/index) и рабочих процессов разработки.</span><span class="sxs-lookup"><span data-stu-id="66a7f-116">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="66a7f-117">Облачная [система конфигурации](xref:fundamentals/configuration) на основе среды.</span><span class="sxs-lookup"><span data-stu-id="66a7f-117">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration).</span></span>
* <span data-ttu-id="66a7f-118">Встроенное [введение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="66a7f-118">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="66a7f-119">Упрощенный, высокопроизводительный и модульный конвейер HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="66a7f-119">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="66a7f-120">Возможность размещения в IIS или в собственном процессе.</span><span class="sxs-lookup"><span data-stu-id="66a7f-120">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="66a7f-121">Возможность запуска на платформе [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), которая поддерживает параллельное управление версиями приложения.</span><span class="sxs-lookup"><span data-stu-id="66a7f-121">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="66a7f-122">Инструментарий, упрощающий процесс современной веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="66a7f-122">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="66a7f-123">Возможность сборки и запуска в ОС Windows, macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="66a7f-123">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="66a7f-124">Открытый исходный код и ориентация на сообщество.</span><span class="sxs-lookup"><span data-stu-id="66a7f-124">Open-source and community-focused.</span></span>

<span data-ttu-id="66a7f-125">ASP.NET Core поставляется полностью в виде пакетов [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="66a7f-125">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="66a7f-126">Это позволяет оптимизировать приложения для включения только необходимых пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="66a7f-126">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="66a7f-127">За счет небольшого размера контактной зоны приложения доступны такие преимущества, как более высокий уровень безопасности, минимальное обслуживание и улучшенная производительность.</span><span class="sxs-lookup"><span data-stu-id="66a7f-127">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="66a7f-128">Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="66a7f-128">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="66a7f-129">ASP.NET Core MVC предоставляет функциональные возможности, упрощающие создание [веб-API](xref:tutorials/index#building-web-apis) и [веб-приложений](xref:tutorials/index#building-web-applications).</span><span class="sxs-lookup"><span data-stu-id="66a7f-129">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="66a7f-130">[Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения [тестируемыми](testing/index.md).</span><span class="sxs-lookup"><span data-stu-id="66a7f-130">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="66a7f-131">[Страницы Razor](xref:mvc/razor-pages/index) (новинка в версии 2.0) — это основанная на страницах модель программирования, которая упрощает и повышает эффективность создания пользовательского веб-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="66a7f-131">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="66a7f-132">[Синтаксис Razor](xref:mvc/views/razor) предоставляет эффективный язык для [страниц Razor](xref:mvc/razor-pages/index) и [представлений MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="66a7f-132">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="66a7f-133">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="66a7f-133">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="66a7f-134">Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](mvc/models/formatting.md) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="66a7f-134">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="66a7f-135">[Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросы с параметрами метода действия.</span><span class="sxs-lookup"><span data-stu-id="66a7f-135">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="66a7f-136">[Проверка модели](xref:mvc/models/validation) автоматически выполняет проверку на стороне сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="66a7f-136">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="66a7f-137">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="66a7f-137">Client-side development</span></span>

<span data-ttu-id="66a7f-138">ASP.NET Core легко интегрируется с широким спектром клиентских платформ, включая [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) и [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="66a7f-138">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="66a7f-139">Дополнительные сведения см. в разделе о [клиентской разработке](client-side/index.md).</span><span class="sxs-lookup"><span data-stu-id="66a7f-139">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66a7f-140">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="66a7f-140">Next steps</span></span>

<span data-ttu-id="66a7f-141">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="66a7f-141">For more information, see the following resources:</span></span>

* [<span data-ttu-id="66a7f-142">Учебники по ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66a7f-142">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="66a7f-143">Основы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66a7f-143">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="66a7f-144">[В еженедельном выпуске ASP.NET community standup](https://live.asp.net/) рассматривается ход работы и планы команды, а также публикуются новые блоги и стороннее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="66a7f-144">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
