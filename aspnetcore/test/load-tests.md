---
title: Нагрузочное тестирование в ASP.NET Core
author: Jeremy-Meng
description: Узнайте о нескольких важных средствах и подходах к нагрузочному тестированию приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 1fd77a767fb53b9276081dd712e13108094a0382
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78649642"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="668d5-103">Нагрузочное тестирование в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="668d5-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="668d5-104">Нагрузочное тестирование и нагрузочные испытания важны для обеспечения эффективности и масштабируемости веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="668d5-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="668d5-105">Их цели различаются, несмотря на то, что они часто используют аналогичные тесты.</span><span class="sxs-lookup"><span data-stu-id="668d5-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="668d5-106">**Нагрузочное тестирование** &ndash; позволяет проверить, способно ли приложение управлять заданной нагрузкой пользователей для определенного сценария, сохранив при этом соблюдение цели отклика.</span><span class="sxs-lookup"><span data-stu-id="668d5-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="668d5-107">Приложение запускается в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="668d5-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="668d5-108">**Нагрузочные испытания** &ndash; служат для тестирования стабильности приложений при выполнении в экстремальных условиях, часто в течение длительного периода времени.</span><span class="sxs-lookup"><span data-stu-id="668d5-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="668d5-109">В тестах создается большая нагрузка на пользователей, пиковая или постепенно увеличивающаяся нагрузка на приложение или ограничиваются вычислительные ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="668d5-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="668d5-110">Нагрузочные испытания определяют, может ли приложение под нагрузкой восстанавливаться после сбоя и корректно возвращаться к ожидаемому поведению.</span><span class="sxs-lookup"><span data-stu-id="668d5-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="668d5-111">В условиях нагрузки приложение не запускается в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="668d5-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="668d5-112">Visual Studio 2019 — это последняя версия Visual Studio, включающая средства нагрузочного тестирования.</span><span class="sxs-lookup"><span data-stu-id="668d5-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="668d5-113">Клиентам, которым в будущем потребуются средства нагрузочного тестирования, мы рекомендуем использовать альтернативные средства нагрузочного тестирования, такие как Apache JMeter, Akamai CloudTest или BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="668d5-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="668d5-114">Дополнительные сведения см. в [заметках о выпуске Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="668d5-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="668d5-115">Инструменты Visual Studio</span><span class="sxs-lookup"><span data-stu-id="668d5-115">Visual Studio tools</span></span>

<span data-ttu-id="668d5-116">Visual Studio позволяет пользователям создавать, разрабатывать и отлаживать тесты веб-производительности и нагрузочные тесты.</span><span class="sxs-lookup"><span data-stu-id="668d5-116">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="668d5-117">Имеется параметр для создания тестов путем записи действий в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="668d5-117">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="668d5-118">Сведения о создании, настройке и запуске проектов нагрузочных тестов с помощью Visual Studio 2017 см. в разделе [Быстрое начало. Создание проекта нагрузочного тестирования](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="668d5-118">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="668d5-119">Нагрузочные тесты можно настроить для локального запуска или запуска в облаке с помощью Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="668d5-119">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="668d5-120">Сторонние средства</span><span class="sxs-lookup"><span data-stu-id="668d5-120">Third-party tools</span></span>

<span data-ttu-id="668d5-121">В списке ниже представлены сторонние средства тестирования веб-производительности с различными наборами функций:</span><span class="sxs-lookup"><span data-stu-id="668d5-121">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="668d5-122">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="668d5-122">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="668d5-123">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="668d5-123">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="668d5-124">Gatling</span><span class="sxs-lookup"><span data-stu-id="668d5-124">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="668d5-125">k6</span><span class="sxs-lookup"><span data-stu-id="668d5-125">k6</span></span>](https://k6.io)
* [<span data-ttu-id="668d5-126">Locust</span><span class="sxs-lookup"><span data-stu-id="668d5-126">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="668d5-127">West Wind WebSurge</span><span class="sxs-lookup"><span data-stu-id="668d5-127">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="668d5-128">Netling</span><span class="sxs-lookup"><span data-stu-id="668d5-128">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="668d5-129">Vegeta</span><span class="sxs-lookup"><span data-stu-id="668d5-129">Vegeta</span></span>](https://github.com/tsenart/vegeta)

