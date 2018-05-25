---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Использование SignalR с веб-приложений в службе приложений Azure | Документы Microsoft
author: pfletcher
description: В этом документе описываются способы настройки приложения SignalR, которое выполняется в Microsoft Azure. Версии программного обеспечения используется в учебнике по Visual Studio 2013 или Vis....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Использование SignalR с веб-приложений в службе приложений Azure
====================
по [Патрик Флетчера](https://github.com/pfletcher)

> В этом документе описываются способы настройки приложения SignalR, которое выполняется в Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) или Visual Studio 2012
> - .NET 4.5
> - SignalR версии 2
> - 2.3 пакета Azure SDK для Visual Studio 2013 или 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), или [форумы Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Содержание

- [Введение](#introduction)
- [Развертывание SignalR веб-приложения в службе приложений Azure](#deploying)
- [Включение соединений WebSocket на службы приложений Azure](#websocket)
- [Использование объединительной платы кэша Redis для Azure](#backplane)
- [Следующие шаги](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Вступление

ASP.NET SignalR можно использовать для переноса на новый уровень взаимодействия между серверами и веб- или клиентов .NET. При размещении в Azure приложений SignalR можно воспользоваться преимуществами высокой надежности, масштабируемых и высокопроизводительных среда, в облаке предоставляет.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Развертывание SignalR веб-приложения в службе приложений Azure

SignalR не добавляет всяких осложнений, определенного для развертывания приложения в Azure и развертывание на локальном сервере. Приложение, использующее SignalR может размещаться в Azure без изменений в конфигурации и других параметров (то, что поддержка WebSockets. в разделе [Включение соединений WebSocket на службе приложений Azure](#websocket) ниже.) В этом учебнике вы развернете приложение, созданное в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) в Azure.

**Необходимые компоненты**

- Visual Studio 2013. Если у вас нет Visual Studio, Visual Studio 2013 Express для Web включается в установку пакета Azure SDK.
- [2.3 пакета Azure SDK для Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) или [2.3 пакета Azure SDK для Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Для выполнения заданий данного учебника, вам потребуется подписка Azure. Вы можете [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), или [зарегистрироваться для пробной подписки](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Развертывание веб-приложения SignalR в Azure

1. Завершить [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить завершенного проекта из [коллекции кода](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. В Visual Studio, выберите **построения**, **публикации чат SignalR**.
3. В диалоговом окне «Публикация веб-сайта» выберите «веб-сайтов Windows Azure».

    ![Выберите веб-сайты Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Если вы не вошли свою учетную запись Майкрософт, нажмите кнопку **входа...**  в диалоговое окно «выберите существующий веб-сайт», а вход.

    ![Выберите существующий веб-сайт](using-signalr-with-azure-web-sites/_static/image2.png)    ![Войдите в Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. В диалоговом окне «выберите существующий веб-сайт» выберите **New**.

    ![Новый веб-сайт](using-signalr-with-azure-web-sites/_static/image4.png)
6. В диалоговом окне «Создание сайта в Windows Azure» введите имя уникальным приложения. Выберите регион, ближайший к вам в раскрывающемся списке регион. Нажмите кнопку **Создать**.

    ![Создание сайта в Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. В диалоговом окне «Публикация веб-сайта» выберите **публикации**.

    ![опубликовать сайт](using-signalr-with-azure-web-sites/_static/image6.png)
8. После завершения публикации приложения в браузере откроется чата SignalR приложение, размещенное в веб-приложениях службы приложений Azure.

    ![Узел, открывая в браузере](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Включение соединений WebSocket в веб-приложениях службы приложений Azure

WebSockets необходимо явно включить в веб-приложения для использования в приложении SignalR; в противном случае будут использоваться другие протоколы (см. [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports) подробные сведения).

Чтобы использовать соединения WebSocket на веб-приложениях службы приложений Azure, необходимо включите его в раздел конфигурации веб-приложения. Чтобы сделать это, откройте веб-приложения в [портала управления Azure](https://manage.windowsazure.com/)и выберите пункт Настройка.

![Вкладка "Настройка"](using-signalr-with-azure-web-sites/_static/image8.png)

В верхней части страницы конфигурации убедитесь, .NET 4.5 используется для веб-приложения.

![.NET framework версии 4.5 параметр](using-signalr-with-azure-web-sites/_static/image9.png)

На странице «Конфигурация» в **WebSockets** выберите **на**.

![Параметр WebSockets: на](using-signalr-with-azure-web-sites/_static/image10.png)

В нижней части страницы конфигурации, выберите **Сохранить** для сохранения изменений.

![Сохранить параметры](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Использование объединительной платы кэша Redis для Azure

Если вы используете несколько экземпляров веб-приложения, и пользователи этих экземпляров должны взаимодействовать друг с другом (, чтобы, например, мгновенных сообщений, созданных в один экземпляр может достигать пользователей, подключенных к другим экземплярам), [кэша Redis для Azure Задняя панель](../performance/scaleout-with-redis.md) должен быть реализован в вашем приложении.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о веб-приложений в службе приложений Azure см. в разделе [Обзор веб-приложений](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
