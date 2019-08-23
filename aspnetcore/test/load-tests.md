---
title: ASP.NET Coreное тестирование нагрузки и нагрузочного тестирования
author: Jeremy-Meng
description: Узнайте о нескольких важных средствах и подходах к нагрузочному тестированию и нагрузочному тестированию ASP.NET Core приложений.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975244"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="4c1be-103">ASP.NET Coreное тестирование нагрузки и нагрузочного тестирования</span><span class="sxs-lookup"><span data-stu-id="4c1be-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="4c1be-104">Нагрузочное тестирование и нагрузочное тестирование важны для обеспечения выполнения и масштабирования веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="4c1be-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="4c1be-105">Их цели различаются, несмотря на то, что они часто используют аналогичные тесты.</span><span class="sxs-lookup"><span data-stu-id="4c1be-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="4c1be-106">**Нагрузочные тесты** &ndash; Проверьте, может ли приложение управлять заданной нагрузкой пользователей для определенного сценария, в то же время отвечая цели отклика.</span><span class="sxs-lookup"><span data-stu-id="4c1be-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="4c1be-107">Приложение запускается в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="4c1be-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="4c1be-108">**Нагрузочные тесты** &ndash; Протестируйте стабильность приложения при чрезвычайных условиях, часто в течение длительного периода времени.</span><span class="sxs-lookup"><span data-stu-id="4c1be-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="4c1be-109">Тесты помещают большую нагрузку на пользователей, пиковые или постепенно увеличивающие нагрузку на приложение или ограничивают вычислительные ресурсы приложения.</span><span class="sxs-lookup"><span data-stu-id="4c1be-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="4c1be-110">Стрессовые тесты определяют, может ли приложение под нагрузкой восстанавливаться после сбоя и корректно возвращать ожидаемое поведение.</span><span class="sxs-lookup"><span data-stu-id="4c1be-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="4c1be-111">В условиях нагрузки приложение не запускается в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="4c1be-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="4c1be-112">Visual Studio 2019 — это последняя версия Visual Studio с функциями нагрузочного теста.</span><span class="sxs-lookup"><span data-stu-id="4c1be-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="4c1be-113">Для клиентов, которым требуются средства нагрузочного тестирования в будущем, рекомендуется использовать альтернативные средства, такие как Apache JMeter, Akamai Клаудтест и Блаземетер.</span><span class="sxs-lookup"><span data-stu-id="4c1be-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="4c1be-114">Дополнительные сведения см. в заметках о выпуске [Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="4c1be-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="4c1be-115">Служба тестирования нагрузки в Azure DevOps заканчивается на 2020.</span><span class="sxs-lookup"><span data-stu-id="4c1be-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="4c1be-116">Дополнительные сведения см. в разделе [Окончание срока службы облачного нагрузочного тестирования](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="4c1be-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="4c1be-117">Инструменты Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c1be-117">Visual Studio tools</span></span>

<span data-ttu-id="4c1be-118">Visual Studio позволяет пользователям создавать, разрабатывать и отлаживать тесты веб-производительности и нагрузочных тестов.</span><span class="sxs-lookup"><span data-stu-id="4c1be-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="4c1be-119">Доступен параметр для создания тестов путем записи действий в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="4c1be-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="4c1be-120">Сведения о создании, настройке и запуске проектов нагрузочных тестов с помощью Visual Studio 2017 см. в разделе [краткое руководство. Создание проекта нагрузочного тестирования](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="4c1be-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="4c1be-121">Нагрузочные тесты можно настроить для локального запуска или запуска в облаке с помощью Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="4c1be-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="4c1be-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="4c1be-122">Azure DevOps</span></span>

<span data-ttu-id="4c1be-123">Запуск нагрузочных тестов можно запустить с помощью службы [Test Plans Azure DevOps](/azure/devops/test/load-test/index?view=vsts) .</span><span class="sxs-lookup"><span data-stu-id="4c1be-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Целевая страница нагрузочного тестирования DevOps для Azure](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="4c1be-125">Служба поддерживает следующие форматы тестов:</span><span class="sxs-lookup"><span data-stu-id="4c1be-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="4c1be-126">Веб- &ndash; тест Visual Studio, созданный в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c1be-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="4c1be-127">&ndash; Сохраненный HTTP-трафик в архиве воспроизводится во время тестирования.</span><span class="sxs-lookup"><span data-stu-id="4c1be-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="4c1be-128">[На основе URL-адреса](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Позволяет указывать URL-адреса для нагрузочного теста, типов запросов, заголовков и строк запросов.</span><span class="sxs-lookup"><span data-stu-id="4c1be-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="4c1be-129">Параметры запуска параметров, такие как длительность, шаблон нагрузки и количество пользователей, можно настроить.</span><span class="sxs-lookup"><span data-stu-id="4c1be-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="4c1be-130">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="4c1be-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="4c1be-131">портала Azure</span><span class="sxs-lookup"><span data-stu-id="4c1be-131">Azure portal</span></span>

<span data-ttu-id="4c1be-132">[Портал Azure позволяет настраивать и выполнять нагрузочное тестирование веб-приложений](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) непосредственно с вкладки " **производительность** " службы приложений в портал Azure.</span><span class="sxs-lookup"><span data-stu-id="4c1be-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Служба приложений Azure в портал Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="4c1be-134">Тест может быть ручным тестом с указанным URL-адресом или файлом Web Test Visual Studio, который может проверять несколько URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="4c1be-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Страница "Создание теста производительности" на портал Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="4c1be-136">В конце теста созданные отчеты показывают характеристики производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="4c1be-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="4c1be-137">Пример статистики:</span><span class="sxs-lookup"><span data-stu-id="4c1be-137">Example statistics include:</span></span>

* <span data-ttu-id="4c1be-138">Среднее время отклика</span><span class="sxs-lookup"><span data-stu-id="4c1be-138">Average response time</span></span>
* <span data-ttu-id="4c1be-139">Максимальная пропускная способность: количество запросов в секунду</span><span class="sxs-lookup"><span data-stu-id="4c1be-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="4c1be-140">Процент сбоев</span><span class="sxs-lookup"><span data-stu-id="4c1be-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="4c1be-141">Сторонние инструменты</span><span class="sxs-lookup"><span data-stu-id="4c1be-141">Third-party tools</span></span>

<span data-ttu-id="4c1be-142">Следующий список содержит сторонние веб-инструменты производительности с различными наборами функций:</span><span class="sxs-lookup"><span data-stu-id="4c1be-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="4c1be-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="4c1be-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="4c1be-144">Апачебенч (AB)</span><span class="sxs-lookup"><span data-stu-id="4c1be-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="4c1be-145">гатлинг</span><span class="sxs-lookup"><span data-stu-id="4c1be-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="4c1be-146">локуст</span><span class="sxs-lookup"><span data-stu-id="4c1be-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="4c1be-147">Западная обмотка Ветер</span><span class="sxs-lookup"><span data-stu-id="4c1be-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="4c1be-148">нетлинг</span><span class="sxs-lookup"><span data-stu-id="4c1be-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="4c1be-149">вежета</span><span class="sxs-lookup"><span data-stu-id="4c1be-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
