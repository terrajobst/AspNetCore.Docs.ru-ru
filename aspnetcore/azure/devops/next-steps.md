---
title: DevOps с помощью ASP.NET Core и Azure | Дальнейшие действия
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089884"
---
# <a name="next-steps"></a><span data-ttu-id="3b41e-103">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3b41e-103">Next steps</span></span>

<span data-ttu-id="3b41e-104">В этом руководстве вы создали конвейер DevOps для примера приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b41e-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="3b41e-105">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="3b41e-105">Congratulations!</span></span> <span data-ttu-id="3b41e-106">Мы надеемся, что вам понравился выпуск обучение для публикации веб-приложений ASP.NET Core в службе приложений Azure и автоматизация непрерывной интеграции изменений.</span><span class="sxs-lookup"><span data-stu-id="3b41e-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="3b41e-107">Помимо размещения веб-сайтов и DevOps Azure предлагает широкий набор служб платформы как услуга (PaaS), полезны для разработчиков ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b41e-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="3b41e-108">В этом разделе дается краткий обзор некоторых наиболее часто используемых служб.</span><span class="sxs-lookup"><span data-stu-id="3b41e-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="3b41e-109">Хранилище и базы данных</span><span class="sxs-lookup"><span data-stu-id="3b41e-109">Storage and databases</span></span>

<span data-ttu-id="3b41e-110">[Кэш redis](/azure/redis-cache/) — это кэширование доступно в виде службы данных высокой пропускной способностью и малой задержкой.</span><span class="sxs-lookup"><span data-stu-id="3b41e-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="3b41e-111">Его можно использовать для кэширования вывода страниц, уменьшая запросов к базе данных и предоставление состояния сеанса ASP.NET Core по нескольким экземплярам приложения.</span><span class="sxs-lookup"><span data-stu-id="3b41e-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="3b41e-112">[Служба хранилища Azure](/azure/storage/) — высокомасштабируемое Облачное хранилище Azure.</span><span class="sxs-lookup"><span data-stu-id="3b41e-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="3b41e-113">Разработчики могут воспользоваться преимуществами [хранилища очередей](/azure/storage/queues/storage-queues-introduction) для надежные очереди сообщений, и [хранилище таблиц](/azure/storage/tables/table-storage-overview) — это хранилище ключ значение NoSQL для быстрой разработки с помощью больших полуструктурированных наборов данных.</span><span class="sxs-lookup"><span data-stu-id="3b41e-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="3b41e-114">[База данных SQL Azure](/azure/sql-database/) предоставляет функциональные возможности знакомых реляционной базы данных как услуга на базе ядра Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b41e-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="3b41e-115">[Cosmos DB](/azure/cosmos-db/) глобально распределенная, многомодельная служба базы данных NoSQL.</span><span class="sxs-lookup"><span data-stu-id="3b41e-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="3b41e-116">Доступны несколько API-интерфейсов, включая MongoDB, Cassandra и API SQL (ранее — DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="3b41e-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="3b41e-117">идентификации</span><span class="sxs-lookup"><span data-stu-id="3b41e-117">Identity</span></span>

<span data-ttu-id="3b41e-118">[Azure Active Directory](/azure/active-directory/) и [Azure Active Directory B2C](/azure/active-directory-b2c/) являются обе службы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="3b41e-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="3b41e-119">Azure Active Directory предназначен для корпоративных сценариев и позволяет совместной работы Azure AD B2B (бизнес бизнес), а Azure Active Directory B2C — целевые сценарии бизнес клиент, включая входа социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="3b41e-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="3b41e-120">Мобильный</span><span class="sxs-lookup"><span data-stu-id="3b41e-120">Mobile</span></span>

<span data-ttu-id="3b41e-121">[Концентраторы уведомлений](/azure/notification-hubs/) — это механизм push уведомлений, мультиплатформенную, масштабируемую быстро отправлять миллионы сообщений для приложений, работающих на различных типов устройств.</span><span class="sxs-lookup"><span data-stu-id="3b41e-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="3b41e-122">Веб-инфраструктуру</span><span class="sxs-lookup"><span data-stu-id="3b41e-122">Web infrastructure</span></span>

<span data-ttu-id="3b41e-123">[Служба контейнеров Azure](/azure/aks/) управляет размещенной средой Kubernetes, позволяя быстро и легко развертывать и управлять ими контейнерных приложений без опыта оркестрации контейнеров.</span><span class="sxs-lookup"><span data-stu-id="3b41e-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="3b41e-124">[Поиск Azure](/azure/search/) используется для создания решения корпоративного поиска по частному разнородному контенту.</span><span class="sxs-lookup"><span data-stu-id="3b41e-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="3b41e-125">[Service Fabric](/azure/service-fabric/) — это платформа распределенных систем, которая упрощает процесс упаковки, развертывания и управления масштабируемых и надежных микрослужб и контейнеров.</span><span class="sxs-lookup"><span data-stu-id="3b41e-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
