---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: С помощью SignalR с веб-приложениями в службе приложений Azure | Документация Майкрософт
author: bradygaster
description: В этом документе описываются способы настройки приложения SignalR, которое работает на базе Microsoft Azure. Версии программного обеспечения используется в этом руководстве, Visual Studio 2013 или Vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 13eb5d29a2c40f52aed4b569ec8695f014a05f03
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837706"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>С помощью SignalR с веб-приложениями в службе приложений Azure
====================
по [Патрик Флетчера](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> В этом документе описываются способы настройки приложения SignalR, которое работает на базе Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) или Visual Studio 2012
> - .NET 4.5
> - SignalR версии 2
> - Пакет Azure SDK 2.3 для Visual Studio 2013 или 2012
>
>
>
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
>
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), или [форумы Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Содержание

- [Введение](#introduction)
- [Развертывание SignalR веб-приложения в службе приложений Azure](#deploying)
- [Включение WebSockets в службе приложений Azure](#websocket)
- [Использование объединительной платы кэша redis для Azure](#backplane)
- [Следующие шаги](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Вступление

ASP.NET SignalR можно использовать для переноса на новый уровень взаимодействия между серверами "и" веб "или" клиентов .NET. При размещении в Azure приложений SignalR можно воспользоваться преимуществами высокой доступности, масштабируемый и предоставляет среду с высокой производительностью, работающее в облаке.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Развертывание SignalR веб-приложения в службе приложений Azure

SignalR не добавляет всяких осложнений, определенного с развертыванием приложения в Azure и развертывание на локальном сервере. Приложения, использующего SignalR может размещаться в Azure без изменений в конфигурации и других параметров (хотя поддержка WebSockets, см. в разделе [Включение WebSockets в службе приложений Azure](#websocket) ниже.) В этом руководстве вы развернете приложение, созданное в [Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) в Azure.

**Необходимые компоненты**

- Visual Studio 2013. Если у вас нет Visual Studio, Visual Studio 2013 Express для Web входит в состав установки пакета Azure SDK.
- [Пакет Azure SDK 2.3 для Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) или [2.3 пакета Azure SDK для Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Для работы с этим руководством требуется подписка Azure. Вы можете [активируйте преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), или [зарегистрироваться для получения пробной подписки](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Развертывание веб-приложения SignalR в Azure

1. Завершить [Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить готовый проект из [коллекции исходных кодов](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. В Visual Studio выберите **построения**, **публикации SignalR Chat**.
3. В диалоговом окне «Публикация веб-сайта» выберите «веб-сайтов Windows Azure».

    ![Выберите веб-сайты Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Если вы не вошли учетную запись Майкрософт, нажмите кнопку **вход...**  в диалоговое окно «выберите существующий веб-сайт» и входа.

    ![Выберите существующий веб-сайт](using-signalr-with-azure-web-sites/_static/image2.png)    ![Вход в Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. В диалоговом окне «выберите существующий веб-сайт» нажмите кнопку **New**.

    ![Новый веб-сайт](using-signalr-with-azure-web-sites/_static/image4.png)
6. В диалоговом окне «Создание сайта в Windows Azure» введите уникальное имя. Выберите ближайший к вам регион в раскрывающемся списке регион. Нажмите кнопку **Создать**.

    ![Создание сайта в Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. В диалоговом окне «Публикация веб-сайта» щелкните **публикации**.

    ![Опубликовать сайт](using-signalr-with-azure-web-sites/_static/image6.png)
8. После завершения публикации приложения чата SignalR приложение, размещенное в веб-приложениях службы приложений Azure откроется в браузере.

    ![Узел, открыв в браузере](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Включение WebSockets в веб-приложениях службы приложений Azure

WebSockets необходимо явно включить в веб-приложения для использования в приложении SignalR; в противном случае будет использоваться другие протоколы (см. в разделе [транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports) сведения).

Чтобы использовать WebSocket в веб-приложениях службы приложений Azure, включите его в разделе конфигурации веб-приложения. Чтобы сделать это, откройте веб-приложение в [портала управления Azure](https://manage.windowsazure.com/)и выберите «настроить».

![Вкладка "Настройка"](using-signalr-with-azure-web-sites/_static/image8.png)

В верхней части страницы конфигурации убедитесь, что для веб-приложения используется .NET 4.5.

![.NET framework версии 4.5 параметр](using-signalr-with-azure-web-sites/_static/image9.png)

На странице «Конфигурация» в **WebSockets** выберите **на**.

![Параметр WebSockets: включить](using-signalr-with-azure-web-sites/_static/image10.png)

В нижней части страницы конфигурации, выберите **Сохранить** для сохранения изменений.

![Сохранить параметры](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Использование объединительной платы кэша redis для Azure

Если используется несколько экземпляров для веб-приложения и пользователи этих экземпляров должны взаимодействовать друг с другом (позволяя, например, сообщения чата, созданные в одном экземпляре обращаться пользователей, подключенных к другим экземплярам), [кэша Redis для Azure Задняя панель](../performance/scaleout-with-redis.md) должен быть реализован в приложении.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о веб-приложений в службе приложений Azure, см. в разделе [Обзор веб-приложений](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
