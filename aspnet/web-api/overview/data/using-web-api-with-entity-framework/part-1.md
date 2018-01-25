---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: "Использование веб-API 2 с Entity Framework 6 | Документы Microsoft"
author: MikeWasson
description: "Этот учебник поможет узнать, что основные принципы создания веб-приложения с ASP.NET Web API серверной части. В этом учебнике используется Entity Framework 6 для макета данных..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: cceefa128f90b4c3e23dd31119f44e6ffc55f46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a>Использование веб-API 2 с Entity Framework 6
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

> Этот учебник поможет узнать, что основные принципы создания веб-приложения с ASP.NET Web API серверной части. Учебника используется Entity Framework 6 уровень данных, а Knockout.js для клиентского приложения JavaScript. Учебник также показано, как развернуть приложение на веб-приложениях службы приложений Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2.1
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Этот учебник использует ASP.NET Web API 2 с Entity Framework 6 для создания веб-приложения, который управляет серверную базу данных. Ниже приведен снимок экрана приложения, который будет создан.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Приложение использует конструктор одностраничного приложения (SPA). «Одностраничного приложения» — это общий термин для веб-приложения, которое загружает одной HTML-страницы и затем обновляет страницу динамически, вместо загрузки новой страницы. После загрузки начальной страницы приложение обращается к серверу через запросы AJAX. AJAX запрашивает возвращаемых данных JSON, которые приложение использует для обновления пользовательского интерфейса.

AJAX не новый, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание больших сложных приложений SPA. В этом учебнике используется [Knockout.js](http://knockoutjs.com/), но можно использовать любую платформу клиента JavaScript.

Ниже приведены основные строительные блоки для этого приложения.

- ASP.NET MVC создает HTML-страницы.
- Веб-API ASP.NET обрабатывает запросы AJAX и возвращает данные JSON.
- Knockout.js привязывает данные HTML-элементов для данных JSON.
- Платформа Entity Framework обращается к базе данных.

## <a name="see-this-app-running-on-azure"></a>Это приложение, работающее на платформе Azure см.

Вы действительно хотите см. по завершении узел, работающий как динамические веб-приложения? Можно развернуть полную версию приложения для учетной записи Azure, просто нажав кнопку ниже.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Требуется учетная запись Azure для развертывания этого решения в Azure. Если у вас еще нет учетной записи, у вас есть следующие параметры:

- [Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.
- [Активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.

## <a name="create-the-project"></a>Создание проекта

Запустите Visual Studio. Из **файл** последовательно выберите пункты **New**, а затем выберите **проекта**. (Или нажмите кнопку **новый проект** на начальной странице.)

В **новый проект** диалоговое окно, нажмите кнопку **Web** в левой области и **веб-приложение ASP.NET** в средней области. Назовите проект BookService и нажмите кнопку **ОК**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

В **новый проект ASP.NET** диалогового окна выберите **веб-API** шаблона.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Для размещения проекта в службе приложений Azure, оставьте **узел в облаке** флажок установлен.

Нажмите кнопку **ОК** для создания проекта.

## <a name="configure-azure-settings-optional"></a>Настройте параметры Azure (необязательно)

Если оставить **узлов в облаке** флажок установлен, Visual Studio будет предложено войти в Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

После входа в Azure, Visual Studio предложит настроить веб-приложения. Введите имя для сайта, выберите подписки Azure и выберите географический регион. В разделе **сервера базы данных**выберите **создать новый сервер**. Введите имя пользователя администратора и пароль.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[Вперед](part-2.md)
