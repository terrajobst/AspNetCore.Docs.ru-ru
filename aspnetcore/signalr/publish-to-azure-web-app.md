---
title: Публикация приложения ASP.NET Core SignalR в службе приложений Azure
author: bradygaster
description: Узнайте, как опубликовать приложение ASP.NET Core SignalR в службе приложений Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: d03a007ca883b3d0391b848e3e92c90469ee640a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652654"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Публикация приложения ASP.NET Core SignalR в службе приложений Azure

По [Брейди Гастер](https://twitter.com/bradygaster)

[Служба приложений Azure](/azure/app-service/app-service-web-overview) — это служба платформы [облачных вычислений Майкрософт](https://azure.microsoft.com/) для размещения веб-приложений, включая ASP.NET Core.

> [!NOTE]
> В этой статье описывается публикация приложения ASP.NET Core SignalR из Visual Studio. Дополнительные сведения см. в статье [Служба SignalR для Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Публикация приложения

В этой статье описывается публикация с помощью средств в Visual Studio. Visual Studio Code пользователи могут использовать команды [Azure CLI](/cli/azure) для публикации приложений в Azure. Дополнительные сведения см. в статье [публикация ASP.NET Core приложения в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet).

1. В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.

1. Убедитесь, что в диалоговом окне **Выбор целевого объекта публикации** выбран параметр **служба приложений** и **создать новые** .

1. Выберите **создать профиль** из раскрывающегося списка кнопка " **опубликовать** ".

   Введите сведения, описанные в следующей таблице в диалоговом окне **Создание службы приложений** , и выберите **создать**.

   | Элемент               | Description |
   | ------------------ | ----------- |
   | **Название**           | Уникальное имя приложения. |
   | **подписка**   | Подписка Azure, используемая приложением. |
   | **Группа ресурсов** | Группа связанных ресурсов, к которым принадлежит приложение. |
   | **План размещения**   | Тарифный план для веб-приложения. |

1. Выберите **службу SignalR Azure** в раскрывающемся списке **зависимости** > **добавить** :

   область зависимости ![, в которой отображается Выбор службы SignalR Azure в раскрывающемся списке Добавить](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. В диалоговом окне **службы SignalR Azure** выберите **создать новый экземпляр службы SignalR Azure**.

1. Укажите **имя**, **группу ресурсов**и **Расположение**. Вернитесь в диалоговое окно **службы SignalR Azure** и нажмите кнопку **Добавить**.

Visual Studio выполняет следующие задачи:

* Создает профиль публикации, содержащий параметры публикации.
* Создает *веб-приложение Azure* с предоставленными сведениями.
* Публикует приложение.
* Запускает браузер, который загружает веб-приложение.

Формат URL-адреса приложения `{APP SERVICE NAME}.azurewebsites.net`. Например, приложение с именем `SignalRChatApp` имеет URL-адрес `https://signalrchatapp.azurewebsites.net`.

Если при развертывании приложения, предназначенного для предварительной версии .NET Core, возникает ошибка HTTP *502,2-Bad Gateway* , см. статью [развертывание ASP.NET Core предварительной версии в службе приложений Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) .

## <a name="configure-the-app-in-azure-app-service"></a>Настройка приложения в службе приложений Azure

> [!NOTE]
> *Этот раздел относится только к приложениям, которые не используют службу SignalR Azure.*
>
> Если приложение использует службу SignalR Azure, служба приложений не требует настройки сходства маршрутизации запросов приложений (ARR) и веб-сокетов, описанных в этом разделе. Клиенты подключают свои веб-сокеты к службе SignalR Azure, а не напрямую к приложению.

Для приложений, размещенных без службы Azure SignalR, включите:

* [Сопоставление arr](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) для маршрутизации запросов от пользователя к тому же экземпляру службы приложений. Значение по умолчанию — **On**.
* [Веб-сокеты](xref:fundamentals/websockets) для обеспечения функционирования транспорта веб-сокетов. Значение по умолчанию — **Off**.

1. В портал Azure перейдите к веб-приложению в **службах приложений**.
1. Откройте **конфигурацию** > **Общие параметры**.
1. Установите для **веб-сокетов** значение **вкл**.
1. Убедитесь, что для параметра **сходство arr** установлено значение **вкл**.

## <a name="app-service-plan-limits"></a>Ограничения плана службы приложений

Веб-сокеты и другие транспорты ограничены в зависимости от выбранного плана службы приложений. Дополнительные сведения см. в разделе *ограничения облачных служб Azure* и *ограничения службы приложений* статьи [Подписка Azure и ограничения службы, квоты и ограничения](/azure/azure-subscription-service-limits#app-service-limits) .

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Что такое служба SignalR Azure?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Публикация ASP.NET Core приложения в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet)
* [Размещение и развертывание приложений ASP.NET Core Preview в Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
