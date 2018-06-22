---
title: Публикация ASP.NET Core приложений SignalR на веб-приложение Azure
author: rachelappel
description: Публикация ASP.NET Core приложений SignalR на веб-приложение Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271922"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Публикация ASP.NET Core приложений SignalR на веб-приложение Azure

[Azure веб-приложения](/azure/app-service/app-service-web-overview) — [Microsoft облачные вычисления](https://azure.microsoft.com/) платформы службы для размещения веб-приложений, включая ASP.NET Core.

> [!NOTE]
> В этой статье относится к публикации приложения ASP.NET Core SignalR из Visual Studio. Посетите [SignalR службы для Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) Дополнительные сведения об использовании SignalR в Azure.

## <a name="publish-the-app"></a>Публикация приложения

Visual Studio предоставляет встроенные средства для публикации для веб-приложение Azure. Можно использовать Visual Studio код пользователя [Azure CLI](/cli/azure) команд для публикации приложения в Azure. В этой статье рассказывается о публикации с использованием средства в Visual Studio. Чтобы опубликовать приложение с помощью Azure CLI, в разделе [публикации приложения ASP.NET Core в Azure с помощью средства командной строки](xref:tutorials/publish-to-azure-webapp-using-cli).

Правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации**. Убедитесь, что **создать новый** возврата **выбрать место назначения публикации** диалогового окна, а затем выберите **публикации**.

![Подбор публикации целевой](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Введите следующие сведения в **Создание приложения службы** диалоговое окно и выбрать **создать**.

| Элемент | Описание: |
| ---- | ----------- |
| **Имя приложения** | Уникальное имя приложения. |
| **Подписки** | Подписка Azure, который использует приложение. |
| **Группа ресурсов** | Группа связанных ресурсов, к которым принадлежит приложения.  |
| **План размещения** | Ценовой план для веб-приложения. |

![Создание приложения службы](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio выполняет следующие задачи:

* Создает профиль публикации содержащего параметры публикации.
* Создает или использует существующий *веб-приложения Azure* с помощью указанных сведений.
* Публикация приложения.
* Запускает браузер с загружен опубликованных веб-приложения.

Обратите внимание на формат URL-адреса для приложения является *.azurewebsites {имя приложения} .net*. Например, приложение с именем `SignalRChattR` URL-адрес, который выглядит как `https://signalrchattr.azurewebsites.net`.

См. в случае ошибки HTTP 502.2 [развертывание ASP.NET Core предварительного выпуска в службу приложений Azure](xref:host-and-deploy/azure-apps/index) ее устранения.

## <a name="configure-signalr-web-app"></a>Настройка SignalR веб-приложения

Приложения ASP.NET Core SignalR, которые опубликованы как веб-приложение Azure должно быть [сходства ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) включена. [WebSockets](xref:fundamentals/websockets) для должна быть включена, разрешить передачи WebSockets функции.

На портале Azure перейдите к **параметры приложения** для веб-приложения. Задать **WebSockets** для **на**и проверьте **сходства ARR** — **на**.

![Параметры Azure веб-приложения на портале Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets и других транспортов [ограничены зависят от того, план служб приложений](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Связанные ресурсы

* [Публикация приложения ASP.NET Core в Azure с помощью средств командной строки](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Публикация приложения ASP.NET Core в Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Размещать и развертывать приложения ASP.NET Core Preview в Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
