---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "Обновление проектов SignalR 1.x до версии 2 | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается обновление существующего проекта 1.x SignalR для SignalR 2.x и способы устранения неполадок, которые могут возникнуть во время обновления..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Обновление проектов SignalR 1.x до версии 2
====================
по [Патрик Флетчера](https://github.com/pfletcher)

> В этом разделе описывается обновление существующего проекта 1.x SignalR для SignalR 2.x и способы устранения проблем, которые могут возникнуть во время процесса обновления.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версий 1 и 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>С помощью Visual Studio 2012 с помощью этого учебника
> 
> 
> Чтобы использовать Visual Studio 2012 с помощью этого учебника, выполните следующее:
> 
> - Обновление вашего [диспетчера пакетов](http://docs.nuget.org/docs/start-here/installing-nuget) до последней версии.
> - Установка [веб-установщика платформы](https://www.microsoft.com/web/downloads/platform.aspx).
> - Установщик веб-платформы для поиска и установки **ASP.NET и 2013.1 Web Tools для Visual Studio 2012**. Это будет установки Visual Studio шаблоны для SignalR классов, таких как **концентратора**.
> - Некоторые шаблоны (такие как **класс запуска OWIN**) будет недоступен; в этом случае используйте файл класса.
> 
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 обеспечивает согласованность при работе в серверных платформ с помощью [OWIN](http://owin.org). В этой статье описывается несколько шагов, которые необходимы для обновления приложения 1.x SignalR до версии 2.

Во время его рекомендуется обновить приложения на SignalR 2, SignalR 1.x будет по-прежнему поддерживаться.

Этот учебник описывает обновление приложения на веб сервере 2 SignalR. Резидентных приложений (для которых размещения на сервере в консольном приложении, службы Windows или другой процесс) теперь поддерживаются в группе SignalR 2. Сведения о том, как приступить к созданию резидентного приложения с SignalR 2 см. в разделе [учебника: SignalR резидентной](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Описание

В следующих разделах описаны задачи, связанные с обновлением проектов SignalR и устранение неполадок, которые могут возникнуть.

- [Пример: Учебник для начала обновления до SignalR 2](#example)
- [Устранение ошибок, возникших во время обновления](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Пример: Обновление Приступая к работе учебного приложения для SignalR 2

В этом разделе будет обновлять приложение, созданное в [SignalR версии 1.x учебник по началу работы](../older-versions/index.md) для использования SignalR 2.

1. Закончив учебника Приступая к работе, щелкните правой кнопкой мыши проект и выберите **свойства**. Убедитесь, что **требуемой версии .NET framework** равно **.NET Framework 4.5.**
2. Откройте консоль диспетчера пакетов. Удалить SignalR 1.x из проекта, используя следующую команду:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Установите SignalR 2, используя следующую команду:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. В HTML-страницы обновите ссылку на скрипт для SignalR в соответствии с версией теперь включены в проект скрипта.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. В глобального класса приложения удалите вызов MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Щелкните правой кнопкой мыши решение и выберите **добавить**, **новый элемент...** . В диалоговом окне выберите **класс запуска Owin**. Имя для нового класса **файла Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Замените содержимое файла Startup.cs следующий код:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Добавляет атрибут сборки класса в Owin при запуске процесса, который выполняет `Configuration` метод при запуске Owin. Это в свою очередь вызывает `MapSignalR` метод, который создает маршруты для всех концентраторов SignalR в приложении.
8. Запустите проект и скопируйте URL-адрес главной страницы в другой браузер или область обозревателя, как раньше. Каждая страница будет запрашивать имя пользователя и сообщений, отправленных с каждой страницы должны быть видимыми в обеих областях браузера.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Устранение ошибок, возникших во время обновления

В этом разделе описываются проблемы, которые могут возникнуть во время обновления. Более полный список ошибок и проблем, которые могут возникнуть при использовании приложения SignalR см. в разделе [SignalR Устранение неполадок](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>«Вызов определяется неоднозначно между следующие методы или свойства»

Эта ошибка возникает, если ссылку на `Microsoft.AspNet.SignalR.Owin` не удаляется. Этот пакет является устаревшей; необходимо удалить ссылку, и версии 1.x SelfHost пакета должны быть удалены.

### <a name="hub-methods-fail-silently"></a>Методы концентратора сбоем без уведомления

Убедитесь, что ссылки на скрипты в клиентском приложении до даты и что `OwinStartup` атрибут для запуска класс имеет правильный класс и имя сборки проекта. Кроме того попытайтесь открыть адреса концентраторов (/ signalr/концентраторов), в браузере; любое сообщение об ошибке будет предлагать дополнительные сведения о том, что пошло не так.
