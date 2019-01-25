---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Обновление проектов SignalR 1.x до версии 2 | Документация Майкрософт
author: bradygaster
description: В этом разделе описывается процесс обновления существующего проекта SignalR 1.x для SignalR 2.x и способы устранения неполадок, возникающих в процессе обновления...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: a1cb4903f3cdeef70ffd0f624a3a2170f641a395
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837342"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Обновление проектов SignalR 1.x до версии 2
====================
по [Патрик Флетчера](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> В этом разделе описывается процесс обновления существующего проекта SignalR 1.x для SignalR 2.x и способы устранения неполадок, возникающих в процессе обновления.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR версий 1 и 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>С помощью Visual Studio 2012 с помощью этого руководства
>
>
> Чтобы использовать Visual Studio 2012 с этим руководством, сделайте следующее:
>
> - Обновление вашей [диспетчера пакетов](http://docs.nuget.org/docs/start-here/installing-nuget) до последней версии.
> - Установка [установщик веб-платформы](https://www.microsoft.com/web/downloads/platform.aspx).
> - В установщик веб-платформы, найдите и установите **ASP.NET и Web Tools 2013.1 для Visual Studio 2012**. Это будет установки Visual Studio шаблоны для SignalR классов, таких как **центр**.
> - Некоторые шаблоны (такие как **класс запуска OWIN**) будут недоступны; для этого используйте файл класса.
>
>
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
>
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 предлагает согласованным интерфейсом разработки платформ сервера, на основе [OWIN](http://owin.org). В этой статье описывается несколько шагов, которые необходимы для обновления приложения SignalR 1.x до версии 2.

Хотя мы рекомендуем обновление приложений до SignalR 2, SignalR 1.x будет по-прежнему будет поддерживаться.

Это руководство содержит инструкции для обновления приложения, размещенные в Интернете, SignalR 2. В разделе SignalR 2 теперь поддерживаются резидентных приложений (которые, на которых размещены сервер в консольном приложении, службы Windows или другой процесс). Сведения о том, как приступить к созданию резидентного приложения с помощью SignalR 2, см. в разделе [руководства: Резидентное размещение SignalR](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Описание

Задачи, выполняемые при обновлении проектов SignalR и способы решения проблем, которые могут возникнуть в следующих разделах.

- [Пример: Обновление учебнике Приступая к работе SignalR 2](#example)
- [Устранения ошибок, возникающих во время обновления](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Пример Обновление Приступая к работе учебного приложения SignalR 2

В этом разделе вы обновите приложение, созданное в [SignalR 1.x версию руководства](../older-versions/index.md) с помощью SignalR 2.

1. Когда вы закончите учебник Приступая к работе, правой кнопкой мыши проект и выберите **свойства**. Убедитесь, что **требуемой версии .NET framework** присваивается **.NET Framework 4.5.**
2. Откройте консоль диспетчера пакетов. Удалить SignalR 1.x из проекта, используя следующую команду:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Установите SignalR 2, используя следующую команду:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. В HTML-страницы обновите ссылку на сценарий для SignalR в соответствии с версией теперь включены в проект скрипта.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. В глобальном классе приложения удалите вызов MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Щелкните правой кнопкой мыши решение и выберите **добавить**, **новый элемент...** . В диалоговом окне выберите **класс запуска Owin**. Назовите новый класс **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Замените содержимое Startup.cs следующим кодом:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Атрибут сборки добавляет в процессе запуска Owin в, выполняющий класс `Configuration` метод при запуске Owin. Это в свою очередь вызывает `MapSignalR` метод, который создает маршруты для всех концентраторов SignalR в приложении.
8. Запустите проект и скопируйте URL-адрес главной страницы в другой браузер или область обозревателя, что и раньше. Каждая страница запросит имя пользователя и сообщений, отправленных из каждой страницы должны быть видимыми в обеих областях браузера.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Устранения ошибок, возникающих во время обновления

В этом разделе описаны проблемы, которые могут возникнуть во время обновления. Более полный список ошибок и проблем, которые могут возникнуть в приложении SignalR, см. в разделе [Устранение неполадок SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>«Вызов определяется неоднозначно между следующие методы или свойства»

Эта ошибка возникает в том случае, если ссылка `Microsoft.AspNet.SignalR.Owin` не удаляется. Этот пакет устарел. ссылка должна удаляться и версии 1.x пакета SelfHost должны быть удалены.

### <a name="hub-methods-fail-silently"></a>Методы концентратора выполнена

Убедитесь, что ссылки на скрипты в клиентском приложении актуальны и который `OwinStartup` атрибут для вашего класса Startup есть правильный класс и имя сборки проекта. Кроме того попробуйте открыть этот адрес концентраторов (/ signalr/центры) в браузере; любое сообщение об ошибке будет предлагать дополнительные сведения о том, что не так.
