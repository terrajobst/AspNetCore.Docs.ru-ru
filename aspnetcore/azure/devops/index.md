---
title: DevOps с ASP.NET Core и Azure
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722553"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="b1fc6-103">DevOps с ASP.NET Core и Azure</span><span class="sxs-lookup"><span data-stu-id="b1fc6-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="b1fc6-104">Руководство по жизненному циклу разработки Azure для .NET</span><span class="sxs-lookup"><span data-stu-id="b1fc6-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="b1fc6-105">В этом руководстве предоставляются основные сведения по созданию жизненного цикла разработки в Azure с помощью инструментов и процессов .NET.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="b1fc6-106">После его прохождения вы сможете наиболее эффективно использовать цепочку инструментов DevOps.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="b1fc6-107">Для кого предназначено это руководство</span><span class="sxs-lookup"><span data-stu-id="b1fc6-107">Who this guide is for</span></span>

<span data-ttu-id="b1fc6-108">Вы должны быть опытным разработчиком ASP.NET (уровень 200–300).</span><span class="sxs-lookup"><span data-stu-id="b1fc6-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="b1fc6-109">Вам не нужно ничего знать об Azure, так как эти сведения есть во введении.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="b1fc6-110">Это руководство может также оказаться полезным для инженеров DevOps, которые преимущественно работают с операциями, а не занимаются разработкой.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="b1fc6-111">Это руководство предназначено для разработчиков Windows.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-111">This guide targets Windows developers.</span></span> <span data-ttu-id="b1fc6-112">Linux и macOS также полностью поддерживаются в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="b1fc6-113">Чтобы адаптировать это руководство для Linux или macOS, смотрите сноски, в которых приводятся характерные отличия.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="b1fc6-114">Темы, которые выходят за рамки этого руководства</span><span class="sxs-lookup"><span data-stu-id="b1fc6-114">What this guide doesn't cover</span></span>

<span data-ttu-id="b1fc6-115">В этом руководстве приводятся рекомендации для разработчиков .NET по сквозному непрерывному развертыванию.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="b1fc6-116">Это не исчерпывающее руководство по Azure, и в нем не рассматриваются подробно API .NET для служб Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="b1fc6-117">Основное внимание уделяется непрерывной интеграции, развертыванию, мониторингу и отладке.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="b1fc6-118">В конце руководства предлагаются рекомендации по дальнейшим действиям.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="b1fc6-119">В число предложений входят службы платформы Azure, полезные для разработчиков ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="b1fc6-120">Содержание руководства</span><span class="sxs-lookup"><span data-stu-id="b1fc6-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="b1fc6-121">Инструменты и файлы для скачивания</span><span class="sxs-lookup"><span data-stu-id="b1fc6-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="b1fc6-122">Вы узнаете, где получить инструменты, используемые в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="b1fc6-123">Развертывание в службу приложений</span><span class="sxs-lookup"><span data-stu-id="b1fc6-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="b1fc6-124">Разные способы развертывания приложения ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="b1fc6-125">Непрерывная интеграция и развертывание</span><span class="sxs-lookup"><span data-stu-id="b1fc6-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="b1fc6-126">Создание решения сквозной непрерывной интеграции и развертывания для приложения ASP.NET Core с помощью GitHub, VSTS и Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="b1fc6-127">Мониторинг и отладка</span><span class="sxs-lookup"><span data-stu-id="b1fc6-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="b1fc6-128">Мониторинг, устранение неполадок и настройка приложения с помощью инструментов Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="b1fc6-129">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="b1fc6-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="b1fc6-130">Другие способы изучения Azure для разработчиков ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="b1fc6-131">Благодарности</span><span class="sxs-lookup"><span data-stu-id="b1fc6-131">Acknowledgments</span></span>

<span data-ttu-id="b1fc6-132">Спасибо всем участникам сообщества .NET, которые внесли вклад в создание этого руководства!</span><span class="sxs-lookup"><span data-stu-id="b1fc6-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="b1fc6-133">Мы бы хотели особенно поблагодарить следующих участников сообщества, которые помогли с финальной проверкой материала:</span><span class="sxs-lookup"><span data-stu-id="b1fc6-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="b1fc6-134">Сэм Вронски (Sam Wronski)</span><span class="sxs-lookup"><span data-stu-id="b1fc6-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="b1fc6-135">Джеффри Палермо (Jeffrey Palermo)</span><span class="sxs-lookup"><span data-stu-id="b1fc6-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="b1fc6-136">Заключение</span><span class="sxs-lookup"><span data-stu-id="b1fc6-136">Conclusion</span></span>

<span data-ttu-id="b1fc6-137">Это руководство поможет подготовиться к созданию жизненного цикла непрерывной интеграции и разработки в ASP.NET Core и службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fc6-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="b1fc6-138">Дополнительные материалы для чтения</span><span class="sxs-lookup"><span data-stu-id="b1fc6-138">Additional reading</span></span>

* [<span data-ttu-id="b1fc6-139">Что такое облачные вычисления?</span><span class="sxs-lookup"><span data-stu-id="b1fc6-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="b1fc6-140">Примеры облачных вычислений</span><span class="sxs-lookup"><span data-stu-id="b1fc6-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="b1fc6-141">Что такое IaaS?</span><span class="sxs-lookup"><span data-stu-id="b1fc6-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="b1fc6-142">Что такое PaaS?</span><span class="sxs-lookup"><span data-stu-id="b1fc6-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
