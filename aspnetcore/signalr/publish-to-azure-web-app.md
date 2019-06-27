---
title: Публикация ASP.NET Core SignalR приложения в службе приложений Azure
author: bradygaster
description: Узнайте, как опубликовать приложение ASP.NET Core SignalR в службе приложений Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406102"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Публикация ASP.NET Core SignalR приложения в службе приложений Azure

По [Brady Gaster](https://twitter.com/bradygaster)

[Служба приложений Azure](/azure/app-service/app-service-web-overview) — [Microsoft облачные вычисления](https://azure.microsoft.com/) службу платформы для размещения веб-приложений, включая ASP.NET Core.

> [!NOTE]
> Эта статья относится к публикации приложения ASP.NET Core SignalR из Visual Studio. Дополнительные сведения см. в разделе [SignalR службы для Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Публикация приложения

В этой статье рассматриваются публикации с помощью средств в Visual Studio. Пользователи Visual Studio Code можно использовать [Azure CLI](/cli/azure) команды, чтобы публиковать приложения в Azure. Дополнительные сведения см. в разделе [публикации приложения ASP.NET Core в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet).

1. В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.

1. Убедитесь, что **службы приложений** и **создать** выбраны в **выберите целевой объект публикации** диалоговое окно.

1. Выберите **создать профиль** из **публикации** кнопку раскрывающегося вниз.

   Введите сведения, описанные в следующей таблице в **создать службу приложений** диалогового окна и выберите **создать**.

   | Элемент               | Описание |
   | ------------------ | ----------- |
   | **Name**           | Уникальное имя приложения. |
   | **Подписка**   | Подписка Azure, используемую приложением. |
   | **Группа ресурсов** | Группа связанных ресурсов, к которым относится приложение. |
   | **План размещения**   | Тарифный план для веб-приложения. |

1. Выберите **служба Azure SignalR** в **зависимости** > **добавить** стрелку раскрывающегося списка:

   ![Область зависимостей, показывающий выбор служба Azure SignalR в раскрывающемся списке Добавить](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. В **служба Azure SignalR** диалоговом окне выберите **создания нового экземпляра служба Azure SignalR**.

1. Укажите **имя**, **группы ресурсов**, и **расположение**. Вернитесь к **служба Azure SignalR** диалогового окна и выберите **добавить**.

Visual Studio выполняет следующие задачи:

* Создает профиль публикации содержащий параметры публикации.
* Создает *веб-приложения Azure* с помощью указанных сведений.
* Публикует приложение.
* Запускает браузер, который загружает веб-приложения.

Формат URL-адреса приложения `{APP SERVICE NAME}.azurewebsites.net`. Например, приложение с именем `SignalRChatApp` имеет URL-адрес в `https://signalrchatapp.azurewebsites.net`.

Если HTTP *502.2 - Неверный шлюз* ошибка возникает при развертывании приложение, предназначенное для предварительной версии выпуска .NET Core, см. в разделе [предварительной версии развертывание ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) ее устранения.

## <a name="configure-the-app-in-azure-app-service"></a>Настройка приложения в службе приложений Azure

> [!NOTE]
> *Этот раздел относится только к приложениям, не используется служба Azure SignalR.*
>
> Если приложение использует служба Azure SignalR, службы приложений не требует конфигурации Affinity маршрутизации запросов приложений (ARR) и веб-сокеты, описанные в этом разделе. Клиенты подключаются их веб-сокеты, чтобы служба Azure SignalR, не напрямую к приложению.

Для приложений, размещенных без служба Azure SignalR включите:

* [ARR сходства](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) перенаправлять запросы от пользователя обратно в тот же экземпляр службы приложений. Значение по умолчанию — **на**.
* [Веб-сокеты](xref:fundamentals/websockets) чтобы разрешить веб-сокеты транспорт для функции. Значение по умолчанию — **Off**.

1. На портале Azure перейдите к веб-приложения в **службы приложений**.
1. Откройте **конфигурации** > **Общие параметры**.
1. Задайте **веб-сокеты** для **на**.
1. Убедитесь, что **сходства ARR** присваивается **на**.

## <a name="app-service-plan-limits"></a>Ограничения плана службы приложений

Веб-сокеты и других транспортов ограничиваются зависимости от плана службы приложений выбран. Дополнительные сведения см. в разделе *облачных служб Azure ограничивает* и *ограничения службы приложений* разделы [подписки Azure и ограничения служб, квоты и ограничения](/azure/azure-subscription-service-limits#app-service-limits) статья.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Что такое служба Azure SignalR](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Публикация приложения ASP.NET Core в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet)
* [Размещение и развертывание приложений ASP.NET Core Preview в Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
