---
title: DevOps с помощью ASP.NET Core и Azure | Дальнейшие действия
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "42909678"
---
# <a name="next-steps"></a>Следующие шаги

В этом руководстве вы создали конвейер DevOps для примера приложения ASP.NET Core. Поздравляем! Мы надеемся, что вам понравился выпуск обучение для публикации веб-приложений ASP.NET Core в службе приложений Azure и автоматизация непрерывной интеграции изменений.

Помимо размещения веб-сайтов и DevOps Azure предлагает широкий набор служб платформы как услуга (PaaS), полезны для разработчиков ASP.NET Core. В этом разделе дается краткий обзор некоторых наиболее часто используемых служб.

## <a name="storage-and-databases"></a>Хранилище и базы данных

[Кэш redis](https://docs.microsoft.com/azure/redis-cache/) — это кэширование доступно в виде службы данных высокой пропускной способностью и малой задержкой. Его можно использовать для кэширования вывода страниц, уменьшая запросов к базе данных и предоставление состояния сеанса ASP.NET Core по нескольким экземплярам приложения.

[Служба хранилища Azure](https://docs.microsoft.com/azure/storage/) — высокомасштабируемое Облачное хранилище Azure. Разработчики могут воспользоваться преимуществами [хранилища очередей](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) для надежные очереди сообщений, и [хранилище таблиц](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) — это хранилище ключ значение NoSQL для быстрой разработки с помощью больших полуструктурированных наборов данных.

[База данных SQL Azure](https://docs.microsoft.com/azure/sql-database/) предоставляет функциональные возможности знакомых реляционной базы данных как услуга на базе ядра Microsoft SQL Server.

[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) глобально распределенная, многомодельная служба базы данных NoSQL. Доступны несколько API-интерфейсов, включая MongoDB, Cassandra и API SQL (ранее — DocumentDB).

## <a name="identity"></a>идентификации

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) и [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) являются обе службы удостоверений. Azure Active Directory предназначен для корпоративных сценариев и позволяет совместной работы Azure AD B2B (бизнес бизнес), а Azure Active Directory B2C — целевые сценарии бизнес клиент, включая входа социальных сетей.

## <a name="mobile"></a>Мобильный

[Концентраторы уведомлений](https://docs.microsoft.com/azure/notification-hubs/) — это механизм push уведомлений, мультиплатформенную, масштабируемую быстро отправлять миллионы сообщений для приложений, работающих на различных типов устройств.

## <a name="web-infrastructure"></a>Веб-инфраструктуру

[Служба контейнеров Azure](https://docs.microsoft.com/azure/aks/) управляет размещенной средой Kubernetes, позволяя быстро и легко развертывать и управлять ими контейнерных приложений без опыта оркестрации контейнеров.

[Поиск Azure](https://docs.microsoft.com/azure/search/) используется для создания решения корпоративного поиска по частному разнородному контенту.

[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) — это платформа распределенных систем, которая упрощает процесс упаковки, развертывания и управления масштабируемых и надежных микрослужб и контейнеров.
