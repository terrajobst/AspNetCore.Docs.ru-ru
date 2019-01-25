---
title: Публикация ASP.NET Core SignalR приложение веб-приложении Azure
author: bradygaster
description: Публикация ASP.NET Core SignalR приложение веб-приложении Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837680"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Публикация ASP.NET Core SignalR приложения для веб-приложение Azure

[Веб-приложение Azure](/azure/app-service/app-service-web-overview) — [Microsoft облачные вычисления](https://azure.microsoft.com/) службу платформы для размещения веб-приложений, включая ASP.NET Core.

> [!NOTE]
> Эта статья относится к публикации приложения ASP.NET Core SignalR из Visual Studio. Посетите [SignalR службы для Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) Дополнительные сведения об использовании SignalR в Azure.

## <a name="publish-the-app"></a>Публикация приложения

Visual Studio предоставляет встроенные средства для публикации в веб-приложение Azure. Visual Studio Code пользователь может использовать [Azure CLI](/cli/azure) команды, чтобы публиковать приложения в Azure. В этой статье рассматриваются публикации с помощью средств в Visual Studio. Чтобы опубликовать приложение с помощью Azure CLI, см. в разделе [публикации приложения ASP.NET Core в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet).

Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации**. Убедитесь, что **создать** возвратом **выберите целевой объект публикации** диалоговое окно, а затем выберите **публикации**.

![Выбор целевого объекта публикации](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Введите следующие сведения в **создать службу приложений** диалогового окна и выберите **создать**.

| Элемент | Описание |
| ---- | ----------- |
| **Имя приложения** | Уникальное имя приложения. |
| **Подписка** | Подписка Azure, используемую приложением. |
| **Группа ресурсов** | Группа связанных ресурсов, к которым относится приложение.  |
| **План размещения** | Тарифный план для веб-приложения. |

![Создание службы приложений](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio выполняет следующие задачи:

* Создает профиль публикации содержащий параметры публикации.
* Создает или использует существующий *веб-приложения Azure* с помощью указанных сведений.
* Публикует приложение.
* Запускает браузер, с помощью опубликованного веб-приложения, которые загружены.

Обратите внимание на формат URL-адрес для приложения — *{имя_приложения} .azurewebsites .net*. Например, приложение с именем `SignalRChattR` имеет URL-адрес, который выглядит как `https://signalrchattr.azurewebsites.net`.

Если возникает ошибка HTTP 502.2, см. в разделе [предварительной версии развертывание ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/index) ее устранения.

## <a name="configure-signalr-web-app"></a>Настройка веб-приложения SignalR

Приложения ASP.NET Core SignalR, которые опубликованы как веб-приложение Azure необходим [сходства ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) включена. [WebSockets](xref:fundamentals/websockets) должна быть включена, чтобы разрешить WebSockets транспорт для функции.

На портале Azure перейдите к **параметры приложения** для веб-приложения. Задайте **WebSockets** для **на**и проверьте **ARR сходства** является **на**.

![Параметры в Azure веб-приложения на портале Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets и других транспортов [ограничены зависят от того, план службы приложений](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Связанные ресурсы

* [Публикация приложения ASP.NET Core в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet)
* [Публикация приложения ASP.NET Core в Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Размещение и развертывание приложений ASP.NET Core Preview в Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
