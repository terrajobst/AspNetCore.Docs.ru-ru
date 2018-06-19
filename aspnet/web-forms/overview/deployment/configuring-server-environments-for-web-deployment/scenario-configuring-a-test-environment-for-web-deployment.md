---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Сценарий: Настройка тестовой среды для веб-развертывание | Документы Microsoft'
author: jrjlee
description: В этом разделе описан типичный веб сценарий развертывания для разработчиков или тестовую среду и описание задачи, которые необходимо выполнить для настройки удостоверения службы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879859"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="00064-103">Сценарий: Настройка тестовой среды для веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="00064-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="00064-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="00064-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="00064-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="00064-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="00064-106">В этом разделе описан типичный веб сценарий развертывания для разработчиков или тестовую среду и описание задачи, которые необходимо выполнить для настройки среды.</span><span class="sxs-lookup"><span data-stu-id="00064-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="00064-107">Когда разработчики работают над веб-приложений, они часто ей предоставляется доступ к серверной среде, которые можно использовать для проверки изменения в свои приложения в реалистичных условиях.</span><span class="sxs-lookup"><span data-stu-id="00064-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="00064-108">Среды разработки или тестирования этого вида обычно имеет следующие характеристики:</span><span class="sxs-lookup"><span data-stu-id="00064-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="00064-109">Среда состоит из одного веб-сервера и сервер одной базы данных.</span><span class="sxs-lookup"><span data-stu-id="00064-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="00064-110">Разработчики обычно права администратора на серверах, с помощью которых можно настроить среду для требований своих приложений.</span><span class="sxs-lookup"><span data-stu-id="00064-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="00064-111">Внесения изменений в приложения развертываются на регулярной основе, поэтому среда должна поддерживать один шаг или автоматическое развертывание.</span><span class="sxs-lookup"><span data-stu-id="00064-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="00064-112">Например, в нашем [сценарий учебника](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Мэтт Hink — разработчик компании Fabrikam, Inc. Мэтт работает над решением диспетчера контактов, регулярно необходимо развернуть изменения в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="00064-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="00064-113">Мэтт имеет права администратора на веб-сервере и тестовом сервере базу данных.</span><span class="sxs-lookup"><span data-stu-id="00064-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="00064-114">Изначально Мэтт должен иметь возможность развернуть решение в среде тестирования напрямую.</span><span class="sxs-lookup"><span data-stu-id="00064-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="00064-115">Как работает несколько разработчиков и по мере того соединения команда диспетчера контактов, настраивается для непрерывной интеграции (CI) в Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="00064-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="00064-116">Каждый раз, когда разработчик возвращает в содержимом, Team Build следует построить решение, запустите модульные тесты и автоматически развернуть решение в среде тестирования.</span><span class="sxs-lookup"><span data-stu-id="00064-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="00064-117">Обзор решения</span><span class="sxs-lookup"><span data-stu-id="00064-117">Solution Overview</span></span>

<span data-ttu-id="00064-118">Тестовая среда должна поддерживать один шаг или автоматического развертывания с удаленного компьютера, поэтому имеется выбор из двух основных подходов.</span><span class="sxs-lookup"><span data-stu-id="00064-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="00064-119">Можно выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="00064-119">You can:</span></span>

- <span data-ttu-id="00064-120">Настройка тестов веб-сервера для поддержки развертывания с помощью службы агента веб-развертывания («удаленного агента»).</span><span class="sxs-lookup"><span data-stu-id="00064-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="00064-121">Настройка тестов веб-сервера для поддержки развертывания с помощью обработчика веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="00064-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="00064-122">Можно также использовать [развертывание Web по запросу](https://technet.microsoft.com/library/ee517345(WS.10).aspx) («временной агент»).</span><span class="sxs-lookup"><span data-stu-id="00064-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="00064-123">Это похоже на подход с использованием удаленного агента с точки зрения требований и ограничений.</span><span class="sxs-lookup"><span data-stu-id="00064-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="00064-124">В этом случае разработчики имеют права администратора на целевых серверах, и тестовой среды не могут применяться ограничения безопасности, поэтому логичным выбором для тестирования веб-сервер для поддержки развертывания с использованием удаленного агента.</span><span class="sxs-lookup"><span data-stu-id="00064-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="00064-125">Это менее сложна и требует меньше начальной настройки, чем веб-развертывания обработчик подход.</span><span class="sxs-lookup"><span data-stu-id="00064-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="00064-126">Кроме того, потребуется настройка сервера базы данных для поддержки удаленного доступа и развертывания.</span><span class="sxs-lookup"><span data-stu-id="00064-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="00064-127">В этих разделах содержатся сведения, необходимые для выполнения этих задач:</span><span class="sxs-lookup"><span data-stu-id="00064-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="00064-128">[Настройка веб-сервер для публикации (удаленный агент) веб-развертывания](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="00064-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="00064-129">В этом разделе описывается создание веб-сервер, который поддерживает веб-развертывания публикации, используя подход удаленного агента, начиная с Windows Server 2008 R2 чистую сборку.</span><span class="sxs-lookup"><span data-stu-id="00064-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="00064-130">[Настройка сервера базы данных для публикации веб-развертывания](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="00064-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="00064-131">В этом разделе описывается настройка сервера базы данных для поддержки удаленного доступа и развертывания, начиная с установки по умолчанию SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="00064-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="00064-132">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="00064-132">Further Reading</span></span>

<span data-ttu-id="00064-133">За инструкциями по настройке обычной промежуточной среде. в разделе [сценарий: Настройка в среде промежуточного хранения для развертывания веб-](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="00064-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="00064-134">Рекомендации по настройке обычной рабочей среде см. в разделе [сценарий: Настройка рабочей среде для развертывания веб-](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="00064-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00064-135">[Назад](choosing-the-right-approach-to-web-deployment.md)
> [Вперед](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="00064-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
