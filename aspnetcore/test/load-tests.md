---
title: ASP.NET Core нагрузки/нагрузочное тестирование
author: Jeremy-Meng
description: Описывает несколько важные средства и способы нагрузочное тестирование и нагрузочное тестирование приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 08c4251059b7d9f4549ad710054d8299c4943465
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419385"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="78909-103">Нагрузочное тестирование ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78909-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="78909-104">Нагрузочное тестирование и нагрузочное тестирование, важно убедиться, что веб-приложения обеспечивает высокую производительность и масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="78909-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="78909-105">Поставленных целей отличаются, несмотря на то, что они часто имеют похожих тестов.</span><span class="sxs-lookup"><span data-stu-id="78909-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="78909-106">**Нагрузочные тесты**: Проверяет, является ли приложение могло обработать указанный нагрузки пользователей для определенного сценария, поддерживая их ответа.</span><span class="sxs-lookup"><span data-stu-id="78909-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="78909-107">Приложение выполняется в обычных условиях.</span><span class="sxs-lookup"><span data-stu-id="78909-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="78909-108">**Нагрузочные тесты**: Тесты стабильности приложения, при выполнении в экстремальных условиях и часто длительного периода времени:</span><span class="sxs-lookup"><span data-stu-id="78909-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="78909-109">Высокая нагрузка — пики или постепенно увеличивается.</span><span class="sxs-lookup"><span data-stu-id="78909-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="78909-110">Ограниченные вычислительные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="78909-110">Limited computing resources.</span></span>

<span data-ttu-id="78909-111">Под нагрузкой можно его восстановление после сбоя и корректно вернуться к ожидаемое поведение?</span><span class="sxs-lookup"><span data-stu-id="78909-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="78909-112">Под нагрузкой, приложение будет *не* выполнения в нормальных условиях.</span><span class="sxs-lookup"><span data-stu-id="78909-112">Under stress, the app is *not* run under normal conditions.</span></span>

<span data-ttu-id="78909-113">Visual Studio 2019 — это последняя версия Visual Studio, включающая средства нагрузочного тестирования.</span><span class="sxs-lookup"><span data-stu-id="78909-113">Visual Studio 2019 will be the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="78909-114">Клиентам, которым требуются средства нагрузочного тестирования, мы рекомендуем использовать альтернативные средства нагрузочного тестирования, такие как Apache JMeter, Akamai CloudTest или Blazemeter.</span><span class="sxs-lookup"><span data-stu-id="78909-114">For customers requiring load testing tools, we recommend using alternate load testing tools such as Apache JMeter, Akamai CloudTest, Blazemeter.</span></span> <span data-ttu-id="78909-115">Дополнительные сведения см. в разделе [Visual Studio 2019 заметки о выпуске Preview](/visualstudio/releases/2019/release-notes-preview#test-tools).</span><span class="sxs-lookup"><span data-stu-id="78909-115">For more information, see the [Visual Studio 2019 Preview Release Notes](/visualstudio/releases/2019/release-notes-preview#test-tools).</span></span>

<span data-ttu-id="78909-116">Службы нагрузочного тестирования в Azure DevOps заканчивается через 2020 г.</span><span class="sxs-lookup"><span data-stu-id="78909-116">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="78909-117">Дополнительные сведения см. в разделе [Облачное нагрузочное тестирование службы конца своего жизненного цикла](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="78909-117">For more information see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="78909-118">Инструменты Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78909-118">Visual Studio Tools</span></span>

<span data-ttu-id="78909-119">Visual Studio позволяет пользователям создавать, разрабатывать и отлаживать веб-производительности и нагрузочных тестов.</span><span class="sxs-lookup"><span data-stu-id="78909-119">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="78909-120">Доступно для создания тестов путем записи действий в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="78909-120">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="78909-121">[Краткое руководство. Создание проекта нагрузочного тестирования](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) показано, как создание, Настройка и Запуск нагрузочного теста с помощью Visual Studio 2017 проектов.</span><span class="sxs-lookup"><span data-stu-id="78909-121">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="78909-122">См. [Дополнительные ресурсы](#add).</span><span class="sxs-lookup"><span data-stu-id="78909-122">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="78909-123">Нагрузочные тесты можно настроить для запуска в локальном или запуска в облаке, с помощью DevOps в Azure.</span><span class="sxs-lookup"><span data-stu-id="78909-123">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="78909-124">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="78909-124">Azure DevOps</span></span>

<span data-ttu-id="78909-125">Нагрузочных тестов можно запустить с помощью [Azure DevOps планов тестирования](/azure/devops/test/load-test/index?view=vsts) службы.</span><span class="sxs-lookup"><span data-stu-id="78909-125">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Целевая страница тестирования нагрузки Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="78909-127">Служба поддерживает следующие типы формата теста:</span><span class="sxs-lookup"><span data-stu-id="78909-127">The service supports the following types of test format:</span></span>

* <span data-ttu-id="78909-128">Тест Visual Studio — веб-теста, созданных в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78909-128">Visual Studio test – web test created in Visual Studio.</span></span>
* <span data-ttu-id="78909-129">Тестов на основе архива HTTP — записанный HTTP-трафик в архив воспроизводится во время тестирования.</span><span class="sxs-lookup"><span data-stu-id="78909-129">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="78909-130">[Тестов на основе URL-адрес](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) — позволяет указать URL-адреса, чтобы загрузить тест, типы запросов, заголовки и строки запросов.</span><span class="sxs-lookup"><span data-stu-id="78909-130">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="78909-131">Запустить задание параметров, таких как длительность, шаблон нагрузки, пользователей и т. д., можно настроить.</span><span class="sxs-lookup"><span data-stu-id="78909-131">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
* <span data-ttu-id="78909-132">[Apache JMeter](https://jmeter.apache.org/) тестирования.</span><span class="sxs-lookup"><span data-stu-id="78909-132">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="78909-133">порталу Azure</span><span class="sxs-lookup"><span data-stu-id="78909-133">Azure portal</span></span>

<span data-ttu-id="78909-134">[Портал Azure предоставляет настройки и выполнения нагрузочного тестирования веб-приложений,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) напрямую из вкладки производительности службы приложений на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="78909-134">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![Служба приложений Azure на портале Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="78909-136">Тест может быть ручного теста с указанным URL-адрес или файл веб-тест Visual Studio, который можно проверить несколько URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="78909-136">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Новая страница тестирования производительности на портале Azure.](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="78909-138">В конце теста отчеты создаются для отображения характеристик производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="78909-138">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="78909-139">Пример Статистика включает в себя:</span><span class="sxs-lookup"><span data-stu-id="78909-139">Example statistics include:</span></span>

* <span data-ttu-id="78909-140">Среднее время отклика</span><span class="sxs-lookup"><span data-stu-id="78909-140">Average response time</span></span>
* <span data-ttu-id="78909-141">Макс. пропускная способность: запросов в секунду</span><span class="sxs-lookup"><span data-stu-id="78909-141">Max throughput: requests per second</span></span>
* <span data-ttu-id="78909-142">Процент неудачных</span><span class="sxs-lookup"><span data-stu-id="78909-142">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="78909-143">Сторонние средства</span><span class="sxs-lookup"><span data-stu-id="78909-143">Third-party Tools</span></span>

<span data-ttu-id="78909-144">Следующий список содержит средства обеспечения производительности сторонних разработчиков с различных наборов функций:</span><span class="sxs-lookup"><span data-stu-id="78909-144">The following list contains third-party web performance tools with various feature sets:</span></span>

* <span data-ttu-id="78909-145">[Apache JMeter](https://jmeter.apache.org/) : Полный набор избранные средства тестирования нагрузки.</span><span class="sxs-lookup"><span data-stu-id="78909-145">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="78909-146">Привязана к потоку: требуется один поток для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="78909-146">Thread-bound: need one thread per user.</span></span>
* [<span data-ttu-id="78909-147">AB - сервер Apache HTTP, средство тестирования</span><span class="sxs-lookup"><span data-stu-id="78909-147">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* <span data-ttu-id="78909-148">[Gatling](https://gatling.io/) : Программы рабочего стола с помощью средства графического пользовательского интерфейса и тестирования для записи.</span><span class="sxs-lookup"><span data-stu-id="78909-148">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="78909-149">Более высокую производительность, чем JMeter.</span><span class="sxs-lookup"><span data-stu-id="78909-149">More performant than JMeter.</span></span>
* <span data-ttu-id="78909-150">[Locust.IO](https://locust.io/) : Не ограничено потоков.</span><span class="sxs-lookup"><span data-stu-id="78909-150">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>

## <a name="additional-resources"></a><span data-ttu-id="78909-151">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78909-151">Additional Resources</span></span>

<span data-ttu-id="78909-152">[Серия записей блога теста нагрузки](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) по (Charles Sterling).</span><span class="sxs-lookup"><span data-stu-id="78909-152">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="78909-153">Выпущенное, но большинство из разделов по-прежнему актуальны.</span><span class="sxs-lookup"><span data-stu-id="78909-153">Dated but most of the topics are still relevant.</span></span>
