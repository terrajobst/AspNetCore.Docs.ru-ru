---
title: ASP.NET Core нагрузки/нагрузочное тестирование
author: Jeremy-Meng
description: Дополнительные сведения о нескольких важные средства и способы нагрузочное тестирование и нагрузочное тестирование приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897441"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="25da2-103">ASP.NET Core нагрузки/нагрузочное тестирование</span><span class="sxs-lookup"><span data-stu-id="25da2-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="25da2-104">Нагрузочное тестирование и нагрузочное тестирование, важно убедиться, что веб-приложения обеспечивает высокую производительность и масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="25da2-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="25da2-105">Поставленных целей отличаются, несмотря на то, что они часто имеют похожих тестов.</span><span class="sxs-lookup"><span data-stu-id="25da2-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="25da2-106">**Нагрузочные тесты** &ndash; проверить ли приложение могло обработать указанный нагрузки пользователей для определенного сценария, поддерживая их ответа.</span><span class="sxs-lookup"><span data-stu-id="25da2-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="25da2-107">Приложение выполняется в обычных условиях.</span><span class="sxs-lookup"><span data-stu-id="25da2-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="25da2-108">**Нагрузочные тесты** &ndash; тестовых стабильности приложения, при работе в экстремальных условиях, часто в течение длительного периода времени.</span><span class="sxs-lookup"><span data-stu-id="25da2-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="25da2-109">Тесты поместите высокой пользовательской нагрузки, пики или постепенно увеличение нагрузки на приложение, или они ограничивают вычислительных ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="25da2-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="25da2-110">Нагрузочные тесты определить можно восстановиться после сбоя и корректно вернуться к поведению приложения под нагрузкой.</span><span class="sxs-lookup"><span data-stu-id="25da2-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="25da2-111">Под нагрузкой приложение запускается не в обычных условиях.</span><span class="sxs-lookup"><span data-stu-id="25da2-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="25da2-112">Visual Studio 2019 — это последняя версия Visual Studio с помощью функций нагрузочного тестирования.</span><span class="sxs-lookup"><span data-stu-id="25da2-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="25da2-113">Для клиентов, которым требуется в будущем средства тестирования нагрузки мы рекомендуем альтернативные средства, такие как Apache JMeter, Akamai CloudTest и BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="25da2-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="25da2-114">Дополнительные сведения см. в разделе [заметки о выпуске Visual Studio 2019](/visualstudio/releases/2019/release-notes#test-tools).</span><span class="sxs-lookup"><span data-stu-id="25da2-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="25da2-115">Службы нагрузочного тестирования в Azure DevOps заканчивается через 2020 г.</span><span class="sxs-lookup"><span data-stu-id="25da2-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="25da2-116">Дополнительные сведения см. в разделе [Облачное нагрузочное тестирование службы конца своего жизненного цикла](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="25da2-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="25da2-117">Инструменты Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25da2-117">Visual Studio tools</span></span>

<span data-ttu-id="25da2-118">Visual Studio позволяет пользователям создавать, разрабатывать и отлаживать веб-производительности и нагрузочных тестов.</span><span class="sxs-lookup"><span data-stu-id="25da2-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="25da2-119">Доступно для создания тестов путем записи действий в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="25da2-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="25da2-120">Сведения о том, как создание, Настройка и Запуск нагрузочного теста с помощью Visual Studio 2017 проектов, см. в разделе [краткое руководство: Создание проекта нагрузочного тестирования](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="25da2-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="25da2-121">Дополнительные сведения см. в разделе [дополнительные ресурсы](#additional-resources) раздел.</span><span class="sxs-lookup"><span data-stu-id="25da2-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="25da2-122">Нагрузочные тесты можно настроить для запуска на локальном или выполнения в облаке, с помощью DevOps в Azure.</span><span class="sxs-lookup"><span data-stu-id="25da2-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="25da2-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="25da2-123">Azure DevOps</span></span>

<span data-ttu-id="25da2-124">Нагрузочных тестов можно запустить с помощью [Azure DevOps планов тестирования](/azure/devops/test/load-test/index?view=vsts) службы.</span><span class="sxs-lookup"><span data-stu-id="25da2-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Целевая страница тестирования нагрузки Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="25da2-126">Служба поддерживает следующие форматы теста:</span><span class="sxs-lookup"><span data-stu-id="25da2-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="25da2-127">Visual Studio &ndash; веб-теста, созданных в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25da2-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="25da2-128">Архив HTTP &ndash; записанных HTTP-трафика в архив воспроизводится во время тестирования.</span><span class="sxs-lookup"><span data-stu-id="25da2-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="25da2-129">[URL-адресами](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; позволяет указать URL-адреса, чтобы загрузить тест, типы запросов, заголовки и строки запросов.</span><span class="sxs-lookup"><span data-stu-id="25da2-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="25da2-130">Запустить задание параметров, таких как длительность, шаблон нагрузки и количество пользователей можно настроить.</span><span class="sxs-lookup"><span data-stu-id="25da2-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="25da2-131">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="25da2-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="25da2-132">порталу Azure</span><span class="sxs-lookup"><span data-stu-id="25da2-132">Azure portal</span></span>

<span data-ttu-id="25da2-133">[Портал Azure предоставляет настройки и выполнения нагрузочного тестирования веб-приложений](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) непосредственно из **производительности** вкладку службы приложений на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="25da2-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Служба приложений Azure на портале Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="25da2-135">Тест может быть ручного теста с указанным URL-адрес или файл веб-тест Visual Studio, который можно проверить несколько URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="25da2-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Новая страница тестирования производительности на портале Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="25da2-137">В конце теста созданных отчетов Показать характеристики производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="25da2-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="25da2-138">Пример Статистика включает в себя:</span><span class="sxs-lookup"><span data-stu-id="25da2-138">Example statistics include:</span></span>

* <span data-ttu-id="25da2-139">Среднее время отклика</span><span class="sxs-lookup"><span data-stu-id="25da2-139">Average response time</span></span>
* <span data-ttu-id="25da2-140">Макс. пропускная способность: запросов в секунду</span><span class="sxs-lookup"><span data-stu-id="25da2-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="25da2-141">Процент неудачных</span><span class="sxs-lookup"><span data-stu-id="25da2-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="25da2-142">Сторонние средства</span><span class="sxs-lookup"><span data-stu-id="25da2-142">Third-party tools</span></span>

<span data-ttu-id="25da2-143">Следующий список содержит средства обеспечения производительности сторонних разработчиков с различных наборов функций:</span><span class="sxs-lookup"><span data-stu-id="25da2-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="25da2-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="25da2-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="25da2-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="25da2-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="25da2-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="25da2-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="25da2-147">Locust</span><span class="sxs-lookup"><span data-stu-id="25da2-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="25da2-148">Западная WebSurge ветра</span><span class="sxs-lookup"><span data-stu-id="25da2-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="25da2-149">Netling</span><span class="sxs-lookup"><span data-stu-id="25da2-149">Netling</span></span>](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a><span data-ttu-id="25da2-150">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="25da2-150">Additional resources</span></span>

* [<span data-ttu-id="25da2-151">Серия записей блога теста нагрузки</span><span class="sxs-lookup"><span data-stu-id="25da2-151">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
