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
# <a name="devops-with-aspnet-core-and-azure"></a>DevOps с ASP.NET Core и Azure

Руководство по жизненному циклу разработки Azure для .NET В этом руководстве предоставляются основные сведения по созданию жизненного цикла разработки в Azure с помощью инструментов и процессов .NET. После его прохождения вы сможете наиболее эффективно использовать цепочку инструментов DevOps.

## <a name="who-this-guide-is-for"></a>Для кого предназначено это руководство

Вы должны быть опытным разработчиком ASP.NET (уровень 200–300). Вам не нужно ничего знать об Azure, так как эти сведения есть во введении. Это руководство может также оказаться полезным для инженеров DevOps, которые преимущественно работают с операциями, а не занимаются разработкой.

Это руководство предназначено для разработчиков Windows. Linux и macOS также полностью поддерживаются в .NET Core. Чтобы адаптировать это руководство для Linux или macOS, смотрите сноски, в которых приводятся характерные отличия.

## <a name="what-this-guide-doesnt-cover"></a>Темы, которые выходят за рамки этого руководства

В этом руководстве приводятся рекомендации для разработчиков .NET по сквозному непрерывному развертыванию. Это не исчерпывающее руководство по Azure, и в нем не рассматриваются подробно API .NET для служб Azure. Основное внимание уделяется непрерывной интеграции, развертыванию, мониторингу и отладке. В конце руководства предлагаются рекомендации по дальнейшим действиям. В число предложений входят службы платформы Azure, полезные для разработчиков ASP.NET.

## <a name="whats-in-this-guide"></a>Содержание руководства

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Инструменты и файлы для скачивания](xref:azure/devops/tools-and-downloads)

Вы узнаете, где получить инструменты, используемые в этом руководстве.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Развертывание в службу приложений](xref:azure/devops/deploy-to-app-service)

Разные способы развертывания приложения ASP.NET Core в службе приложений Azure.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Непрерывная интеграция и развертывание](xref:azure/devops/cicd)

Создание решения сквозной непрерывной интеграции и развертывания для приложения ASP.NET Core с помощью GitHub, VSTS и Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Мониторинг и отладка](xref:azure/devops/monitor)

Мониторинг, устранение неполадок и настройка приложения с помощью инструментов Azure.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Дальнейшие действия](xref:azure/devops/next-steps)

Другие способы изучения Azure для разработчиков ASP.NET Core.

## <a name="acknowledgments"></a>Благодарности

Спасибо всем участникам сообщества .NET, которые внесли вклад в создание этого руководства! Мы бы хотели особенно поблагодарить следующих участников сообщества, которые помогли с финальной проверкой материала:

* [Сэм Вронски (Sam Wronski)](https://www.youtube.com/c/worldofzerodevelopment)
* [Джеффри Палермо (Jeffrey Palermo)](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Заключение

Это руководство поможет подготовиться к созданию жизненного цикла непрерывной интеграции и разработки в ASP.NET Core и службе приложений Azure.

## <a name="additional-reading"></a>Дополнительные материалы для чтения

* [Что такое облачные вычисления?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Примеры облачных вычислений](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [Что такое IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)
* [Что такое PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
