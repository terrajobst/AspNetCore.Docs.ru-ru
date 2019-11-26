---
title: ASP.NET Coreное тестирование нагрузки и нагрузочного тестирования
author: Jeremy-Meng
description: Узнайте о нескольких важных средствах и подходах к нагрузочному тестированию и нагрузочному тестированию ASP.NET Core приложений.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: edaa9e873e8e489f0c560c1736f81358ca1720d0
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289026"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="987e4-103">ASP.NET Coreное тестирование нагрузки и нагрузочного тестирования</span><span class="sxs-lookup"><span data-stu-id="987e4-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="987e4-104">Нагрузочное тестирование и нагрузочное тестирование важны для обеспечения выполнения и масштабирования веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="987e4-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="987e4-105">Их цели различаются, несмотря на то, что они часто используют аналогичные тесты.</span><span class="sxs-lookup"><span data-stu-id="987e4-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="987e4-106">**Нагрузочные тесты** &ndash; проверить, может ли приложение управлять заданной нагрузкой пользователей для определенного сценария, в то же время отвечая цели отклика.</span><span class="sxs-lookup"><span data-stu-id="987e4-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="987e4-107">Приложение запускается в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="987e4-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="987e4-108">**Нагрузочные тесты** &ndash; тестировать стабильность приложений при выполнении в условиях экстремальных условий, часто в течение длительного периода времени.</span><span class="sxs-lookup"><span data-stu-id="987e4-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="987e4-109">Тесты помещают большую нагрузку на пользователей, пиковые или постепенно увеличивающие нагрузку на приложение или ограничивают вычислительные ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="987e4-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="987e4-110">Стрессовые тесты определяют, может ли приложение под нагрузкой восстанавливаться после сбоя и корректно возвращать ожидаемое поведение.</span><span class="sxs-lookup"><span data-stu-id="987e4-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="987e4-111">В условиях нагрузки приложение не запускается в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="987e4-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="987e4-112">Visual Studio 2019 — это последняя версия Visual Studio с функциями нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="987e4-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="987e4-113">Для клиентов, которым требуются средства нагрузочного тестирования в будущем, рекомендуется использовать альтернативные средства, такие как Apache JMeter, Akamai Клаудтест и Блаземетер.</span><span class="sxs-lookup"><span data-stu-id="987e4-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="987e4-114">Дополнительные сведения см. в [заметках о выпуске Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="987e4-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="987e4-115">Инструменты Visual Studio</span><span class="sxs-lookup"><span data-stu-id="987e4-115">Visual Studio tools</span></span>

<span data-ttu-id="987e4-116">Visual Studio позволяет пользователям создавать, разрабатывать и отлаживать тесты веб-производительности и нагрузочных тестов.</span><span class="sxs-lookup"><span data-stu-id="987e4-116">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="987e4-117">Доступен параметр для создания тестов путем записи действий в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="987e4-117">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="987e4-118">Сведения о создании, настройке и запуске проектов нагрузочных тестов с помощью Visual Studio 2017 см. в разделе [Краткое руководство. Создание проекта нагрузочного теста](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="987e4-118">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="987e4-119">Нагрузочные тесты можно настроить для локального запуска или запуска в облаке с помощью Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="987e4-119">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="987e4-120">Средства сторонней разработки</span><span class="sxs-lookup"><span data-stu-id="987e4-120">Third-party tools</span></span>

<span data-ttu-id="987e4-121">Следующий список содержит сторонние веб-инструменты производительности с различными наборами функций:</span><span class="sxs-lookup"><span data-stu-id="987e4-121">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="987e4-122">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="987e4-122">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="987e4-123">Апачебенч (AB)</span><span class="sxs-lookup"><span data-stu-id="987e4-123">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="987e4-124">гатлинг</span><span class="sxs-lookup"><span data-stu-id="987e4-124">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="987e4-125">локуст</span><span class="sxs-lookup"><span data-stu-id="987e4-125">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="987e4-126">Западная обмотка Ветер</span><span class="sxs-lookup"><span data-stu-id="987e4-126">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="987e4-127">нетлинг</span><span class="sxs-lookup"><span data-stu-id="987e4-127">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="987e4-128">вежета</span><span class="sxs-lookup"><span data-stu-id="987e4-128">Vegeta</span></span>](https://github.com/tsenart/vegeta)
