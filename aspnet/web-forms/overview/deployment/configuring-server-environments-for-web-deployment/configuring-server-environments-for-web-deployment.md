---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: "Настройка серверов для веб-развертывания | Документы Microsoft"
author: jrjlee
description: "Этого учебника показано, как настроить серверных сред с одним щелчком, так и для автоматических веб-сайт развертывания и публикации в различных разных сценария..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8a07d283e3e4344e5513152cf760ac90481d9f4b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="configuring-server-environments-for-web-deployment"></a>Настройка серверов для веб-развертывания
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Скачать PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Этого учебника показано, как настроить серверных сред с одним щелчком, так и для автоматических веб-сайт развертывания и публикации в различных сценариях другой. Учебник содержит статьи, которые содержат пошаговые инструкции для выполнения различных задач, таких как Настройка веб-сервера для поддержки конкретных подходов к развертыванию и настройке фермы серверов веб-фермы (WFF), вместе с обзорами сценариям, предоставляющие руководство более высокого уровня начала до конца.
> 
> В этом учебнике используется в сценарии развертывания компании Fabrikam, Inc., описанной в [корпоративного веб-развертывания: Обзор сценария](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) как опорную точку, примеры и сетевой инфраструктуры.
> 
> Итальянский преобразования этих учебников, посетите [http://www.lucamorelli.it](http://www.lucamorelli.it).


Этот учебник содержит следующие темы:

- [Выбор правильного подхода к веб-развертывания](choosing-the-right-approach-to-web-deployment.md)
- [Сценарий: Настройка тестовой среды для веб-развертывания](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Сценарий: Настройка среды промежуточного хранения для веб-развертывания](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Сценарий: Настройка рабочей среде для веб-развертывания](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Настройка веб-сервер для публикации (удаленный агент) веб-развертывания](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Настройка веб-сервера для веб-развертывания публикации (веб-развертывания обработчик)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Настройка веб-сервер для публикации (автономного развертывания) веб-развертывания](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Настройка сервера базы данных для публикации веб-развертывания](configuring-a-database-server-for-web-deploy-publishing.md)
- [Создание фермы серверов с помощью платформы веб-фермы](creating-a-server-farm-with-the-web-farm-framework.md)
- [Настройка свойств развертывания для целевой среды](configuring-deployment-properties-for-a-target-environment.md)

Первый раздел [Выбор оптимального подхода справа для развертывания веб-приложения](choosing-the-right-approach-to-web-deployment.md), описание основных подхода, можно использовать для публикации веб-приложений с помощью средство Internet Information Services (IIS) веб-развертывания (Web Deploy) 2.0. Он также определяет сценарии, которые сопоставляются с каждого подхода. Здесь каждый раздел сценария предоставляет высокоуровневый обзор задач, которые необходимо выполнить и выявляет разделы, необходимые для работы через помогут вам выполнить следующие задачи.

Если вы используете разбиение проекта файл подход, описанный в [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md) для построения и развертывания решения, в последнем разделе описывается [настройки свойств развертывания для целевой среды](configuring-deployment-properties-for-a-target-environment.md), описываются способы настройки файлов проекта среды для развертывания в другой целевой среды.

## <a name="key-technologies"></a>Основные технологии

Этот учебник посвящен использовать эти продукты и технологии для поддержки развертывания веб-приложения:

- IIS 7.5
- Веб-развертывание 2.x
- WFF 2.x
- Службы IIS веб-управления (WMSvc)

При использовании Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 и ASP.NET MVC 3 также упоминаются учебника.

## <a name="other-tutorials-in-this-series"></a>Другие учебники по этой серии

Это входит в состав последовательности пять учебники на веб-развертывания корпоративного уровня. Ниже перечислены другие учебники ряда:

- [Развертывание веб-приложений в корпоративных сценариях](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Это вводная содержимое предоставляет контекстные фона для учебника ряда. Он описывает сценарий учебника, и показано, как задачи и пошаговые руководства, описанные на протяжении ряда помещаются в более широкого процесса управления жизненным циклом приложений (ALM).
- [Веб-развертывания на предприятии](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Этот учебник содержит общие сведения о Microsoft Build Engine (MSBuild) файлы проекта, конвейера публикации в Интернете, веб-развертывания и других связанных технологий. Объясняется, как можно использовать эти средства вместе Управление процессами развертывания сложной системы.
- [Настройка Team Foundation Server для развертывания веб-](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Этот учебник описываются способы настройки Team Foundation Server (TFS) для поддержки различных сценариев развертывания, включая автоматическое развертывание в рамках процесса непрерывной интеграции (CI) и вручную активации развертываний определенные сборки.
- [Расширенный корпоративного веб-развертывания](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Этот учебник описывает, как выполнять различные более сложных задач развертывания, таких как Настройка развертывания базы данных для нескольких сред, за исключением файлов и папок из развертывания и получение веб-приложения в автономный режим во время развертывания .

>[!div class="step-by-step"]
[Вперед](choosing-the-right-approach-to-web-deployment.md)
