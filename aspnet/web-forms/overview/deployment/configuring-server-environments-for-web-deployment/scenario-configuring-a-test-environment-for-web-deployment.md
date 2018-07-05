---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Сценарий: Настройка тестовой среды для веб-развертывание | Документация Майкрософт'
author: jrjlee
description: В этом разделе описывается сценарий развертывания обычно web для разработчиков или тестовых сред и описание задачи, которые необходимо выполнить для настройки удостоверения службы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382498"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="f5f65-103">Сценарий: Настройка тестовой среды для веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="f5f65-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="f5f65-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="f5f65-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="f5f65-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="f5f65-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="f5f65-106">В этом разделе описывается сценарий развертывания обычно web для разработчиков или тестовых сред и описание задачи, которые необходимо выполнить для настройки среды.</span><span class="sxs-lookup"><span data-stu-id="f5f65-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="f5f65-107">Когда разработчики работают над веб-приложений, им часто предоставлен доступ к серверной среде, их можно использовать, чтобы проверить изменения в свои приложения в реалистичных условиях.</span><span class="sxs-lookup"><span data-stu-id="f5f65-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="f5f65-108">Такого рода среды разработки или тестирования обычно имеет следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="f5f65-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="f5f65-109">Среда состоит из одного веб-сервера и сервера отдельной базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5f65-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="f5f65-110">Разработчики обычно права администратора на серверах, с помощью которых настроить среду для требования своих приложений.</span><span class="sxs-lookup"><span data-stu-id="f5f65-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="f5f65-111">Внесения изменений в приложения развертываются на регулярной основе, поэтому среда должна поддерживать один шаг или автоматизированное развертывание.</span><span class="sxs-lookup"><span data-stu-id="f5f65-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="f5f65-112">Например, в нашем [сценарий учебника по службам](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Мэтт Hink — разработчик компании Fabrikam, Inc. Мэтт работает над решения диспетчера контактов и нужно регулярно получать список для развертывания изменений в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="f5f65-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="f5f65-113">Мэтт является администратором на веб-сервер тестирования и тестового серверов базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5f65-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="f5f65-114">Изначально Мэтт должен иметь возможность развернуть решение в среде тестирования напрямую.</span><span class="sxs-lookup"><span data-stu-id="f5f65-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="f5f65-115">Как работает продвижения и большему числу разработчиков присоединиться к группе, диспетчер контактов, решение настраивается для непрерывной интеграции (CI) в Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="f5f65-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="f5f65-116">Каждый раз, когда разработчик возвращает в содержимом, Team Build следует построить решение, выполнять все модульные тесты и автоматически развернуть решение в среде тестирования.</span><span class="sxs-lookup"><span data-stu-id="f5f65-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="f5f65-117">Общие сведения о решении</span><span class="sxs-lookup"><span data-stu-id="f5f65-117">Solution Overview</span></span>

<span data-ttu-id="f5f65-118">Тестовая среда должна поддерживать один шаг или автоматизировать развертывание с удаленного компьютера, за счет чего у вас есть выбор из двух основных подходов.</span><span class="sxs-lookup"><span data-stu-id="f5f65-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="f5f65-119">Можно выполнить следующие действия: </span><span class="sxs-lookup"><span data-stu-id="f5f65-119">You can:</span></span>

- <span data-ttu-id="f5f65-120">Настройка веб-сервер тестирования для поддержки развертывания, используя службу агента веб-развертывания («удаленного агента»).</span><span class="sxs-lookup"><span data-stu-id="f5f65-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="f5f65-121">Настройка веб-сервер тестирования для поддержки развертывания, с помощью обработчика веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="f5f65-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="f5f65-122">Можно также использовать [развертывание Web по запросу](https://technet.microsoft.com/library/ee517345(WS.10).aspx) («temp agent»).</span><span class="sxs-lookup"><span data-stu-id="f5f65-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="f5f65-123">Это похоже на удаленный агент подход с точки зрения требования и ограничения.</span><span class="sxs-lookup"><span data-stu-id="f5f65-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="f5f65-124">В этом случае разработчики права администратора на целевых серверах и тестовой среды не распространяются ограничения безопасности, поэтому логичным выбором для тестирования веб-сервер для поддержки развертывания, с помощью удаленного агента.</span><span class="sxs-lookup"><span data-stu-id="f5f65-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="f5f65-125">Это менее сложна и требует меньше начальной настройки, чем подход веб-развертывание обработчик.</span><span class="sxs-lookup"><span data-stu-id="f5f65-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="f5f65-126">Также необходимо настроить сервер базы данных для поддержки удаленного доступа и развертывания.</span><span class="sxs-lookup"><span data-stu-id="f5f65-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="f5f65-127">Эти разделы содержат все сведения, необходимые для выполнения этих задач:</span><span class="sxs-lookup"><span data-stu-id="f5f65-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="f5f65-128">[Настройка веб-сервера для публикации (удаленный агент) веб-развертывания](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="f5f65-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="f5f65-129">В этом разделе описывается создание веб-сервер, который поддерживает веб-развертывания публикации, используя подход удаленного агента, начиная с Windows Server 2008 R2 чистую сборку.</span><span class="sxs-lookup"><span data-stu-id="f5f65-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="f5f65-130">[Настройка сервера базы данных для публикации веб-развертывания](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="f5f65-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="f5f65-131">В этом разделе описывается настройка сервера базы данных для поддержки удаленного доступа и развертывания, начиная с установки по умолчанию SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="f5f65-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="f5f65-132">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="f5f65-132">Further Reading</span></span>

<span data-ttu-id="f5f65-133">Руководство по настройке обычной промежуточной среде, см. в разделе [сценарий: Настройка промежуточной среды для веб-развертывания](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f5f65-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="f5f65-134">Рекомендации по настройке обычной рабочей среды, см. в разделе [сценарий: настройке рабочей среды для веб-развертывания](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f5f65-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f5f65-135">[Назад](choosing-the-right-approach-to-web-deployment.md)
> [Вперед](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="f5f65-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
