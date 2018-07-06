---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Настройка серверных сред для веб-развертывание | Документация Майкрософт
author: jrjlee
description: Этом руководстве показано, как настроить серверных сред с одним щелчком или автоматического развертывания веб-сайта и публикации в различных различных сценария...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: df40cc9d16b9e6b3fe0b659b405106e68cee62b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835567"
---
<a name="configuring-server-environments-for-web-deployment"></a>Настройка серверных сред для веб-развертывания
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Загрузить PDF-файл](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Этом учебнике показано, как настроить серверных сред с одним щелчком или автоматического развертывания веб-сайта и публикации в различных сценариях различных. Учебник включает разделы пошагового выполнения различных задач, например, Настройка веб-сервера для поддержки конкретных подходов к развертыванию и настройке фермы серверов веб-фермы (WFF), а также обзоры на основе сценариев, которые предоставляют руководство более высокого уровня end-to-end.
> 
> В этом руководстве используется сценарий развертывания Fabrikam, Inc., описанный в [корпоративное веб-развертывание: Обзор сценария](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) как точка ссылки, примеры и сетевой инфраструктуры.
> 
> Итальянский перевода учебников, см. в статье [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Этот учебник содержит следующие разделы:

- [Выбор правильного подхода для веб-развертывания](choosing-the-right-approach-to-web-deployment.md)
- [Сценарий. Настройка среды тестирования для веб-развертывания](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Сценарий. Настройка промежуточной среды для веб-развертывания](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Сценарий. Настройка рабочей среды для веб-развертывания](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Настройка веб-сервера для публикации веб-развертывания (удаленный агент)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Настройка веб-сервера для публикации веб-развертывания (обработчик веб-развертывания)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Настройка веб-сервера для публикации веб-развертывания (автономное развертывание)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Настройка сервера базы данных для публикации веб-развертывания](configuring-a-database-server-for-web-deploy-publishing.md)
- [Создание фермы серверов с помощью Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Настройка свойств развертывания для целевой среды](configuring-deployment-properties-for-a-target-environment.md)

Первый раздел [Выбор оптимального подхода для веб-развертывания справа](choosing-the-right-approach-to-web-deployment.md), описание основных подхода, можно использовать для публикации веб-приложений с помощью средство Internet Information Services (IIS) веб-развертывания (Web Deploy) 2.0. Она также определяет сценарии, которые сопоставляются с каждого подхода. На этой странице каждый раздел сценарий представлен общий обзор задач, которые вам потребуется выполнить действия и определяет темы, которые необходимо выполнить для завершения этих задач.

Если вы используете разбиение проекта файл подход, описанный в [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md) для создания и развертывания решения, его последний раздел, [Настройка свойств развертывания для целевой среды](configuring-deployment-properties-for-a-target-environment.md), описывается настройка файлов проекта конкретной среды для развертывания в средах другое назначение.

## <a name="key-technologies"></a>Ключевые технологии

Это руководство посвящено как использовать эти продукты и технологии для поддержки веб-развертывания:

- IIS 7.5
- Веб-развертывания версии 2.x
- WFF 2.x
- Службы IIS веб-управления (WMSvc)

При использовании Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 и ASP.NET MVC 3 также касается руководства.

## <a name="other-tutorials-in-this-series"></a>Другие руководства в этой серии

Это формирует части серии из пяти учебников на веб-развертывание в масштабах предприятия. Ниже приведены другие руководства в серии.

- [Развертывание веб-приложений в корпоративных сценариях](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Вводные представлены контекстные фона для этой серии руководств. Здесь описываются рассматриваемый сценарий, а его показано, как задачи и пошаговые руководства, описанные на протяжении ряда помещается в более широкого процесса управления жизненным циклом приложений (ALM).
- [Веб-развертывания на предприятии](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Этот учебник содержит общие сведения о файлах проекта Microsoft Build Engine (MSBuild), конвейера публикации в Интернете, веб-развертывания и других связанных технологий. Здесь объясняется, как можно использовать эти средства вместе для управления процессами сложного развертывания.
- [Настройка Team Foundation Server для развертывания веб-](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Это руководство содержит инструкции для настройки Team Foundation Server (TFS) для поддержки различных сценариев развертывания, включая автоматическое развертывание в рамках процесса непрерывной интеграции (CI) и запущено развертывание определенных сборок вручную.
- [Advanced корпоративное веб-развертывание](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Это руководство содержит инструкции по выполнению различных задач более сложные развертывания, такие как настройка развертываний баз данных для нескольких сред, исключение файлов и папок из развертывания и получение веб-приложений в автономный режим во время развертывания .

> [!div class="step-by-step"]
> [Вперед](choosing-the-right-approach-to-web-deployment.md)
