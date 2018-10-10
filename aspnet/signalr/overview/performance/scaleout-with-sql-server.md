---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Масштабирование SignalR с помощью SQL Server | Документация Майкрософт
author: MikeWasson
description: Версии программного обеспечения используется в этом разделе 4.5 .NET SignalR для Visual Studio 2013 версии 2, предыдущие версии в этом разделе сведения о более ранних версиях...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7d9acfc8ed2dd692f36528a8a9700d92d86a4203
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910451"
---
<a name="signalr-scaleout-with-sql-server"></a>Масштабирование SignalR с помощью SQL Server
====================
по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR версии 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
>
> Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
>
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


В этом руководстве будет использовать SQL Server, чтобы распределить сообщения в приложении SignalR, которое развертывается в два отдельных экземпляра IIS. Можно также открыть этот пример на одном тестовом компьютере, но чтобы получить полные результаты, необходимо выполнить развертывание приложений SignalR на два или несколько серверов. Также необходимо установить SQL Server на одном из серверов или на отдельном выделенном сервере. Другой вариант — для работы с учебником, с помощью виртуальных машин в Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Предварительные требования

Microsoft SQL Server 2005 или более поздней версии. Задняя панель поддерживает настольных и серверных выпусков SQL Server. Он не поддерживает SQL Server Compact Edition или базы данных SQL Azure. (Если приложение размещено в Azure, рекомендуется использовать на задней стороне служебной шины.)

## <a name="overview"></a>Обзор

Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.

1. Создание новой пустой базы данных. Задняя панель создаст необходимые таблицы в этой базе данных.
2. Добавьте следующие пакеты NuGet приложения:

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Создайте приложение SignalR.
4. Добавьте следующий код в Startup.cs для настройки на задней стороне:

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Этот код настраивает задней панели со значениями по умолчанию для [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) и [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Дополнительные сведения об изменении этих значений см. в разделе [производительность SignalR: метрики горизонтального масштабирования](signalr-performance.md#scaleout_metrics).

## <a name="configure-the-database"></a>Настройка базы данных

Решите, будет ли приложение использовать проверку подлинности Windows или проверка подлинности SQL Server для доступа к базе данных. В любом случае убедитесь, что пользователь базы данных имеет разрешения на вход, создавать схемы и создание таблиц.

Создайте новую базу данных для использования объединительной платы. Можно выбрать любое имя базы данных. Не нужно создавать любой таблицы в базе данных; Задняя панель создаст необходимые таблицы.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Включение компонента Service Broker

Рекомендуется, чтобы включить компонент Service Broker для базы данных объединительной платы. Компонент Service Broker обеспечивает встроенную поддержку для обмена сообщениями и очереди в SQL Server, что позволяет более эффективно получать обновления задней панели. (Однако задней панели также работает без компонента Service Broker).

Чтобы проверить, включен ли компонент Service Broker, запросите **—\_broker\_включена** столбца в **sys.databases** представления каталога.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Чтобы включить компонент Service Broker, используйте следующий запрос SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Если этот запрос принимать участие во взаимоблокировке, убедитесь, что нет подключения к базе данных приложений.


Если трассировка включена, данные трассировки также будет показано, включен ли компонент Service Broker.

## <a name="create-a-signalr-application"></a>Создание приложения SignalR

Создайте приложение SignalR, выполнив одно из следующих руководств:

- [Начало работы с SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Начало работы с SignalR 2.0 и MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Затем мы изменим приложение чата для поддержки горизонтального масштабирования с помощью SQL Server. Во-первых добавьте пакет SignalR.SqlServer NuGet в проект. В Visual Studio из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Затем откройте файл Startup.cs. Добавьте следующий код, чтобы **Настройка** метод:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Развертывание и запуск приложения

Подготовка экземпляров Windows Server для развертывания приложения SignalR.

Добавьте роль IIS. Включить функции «Разработка приложений», включая протокол WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Также службы управления (см. в разделе «Средства управления»).

![](scaleout-with-sql-server/_static/image5.png)

**Установить Web Deploy 3.0.** Если запустить диспетчер служб IIS, он предложит вам установить веб-платформы Майкрософт или вы можете [загрузить intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). В установщике платформы найдите веб-развертывания и установить Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Убедитесь, что веб-службы управления. В противном случае запустите службу. (Если вы не видите веб-службы управления в списке служб Windows, убедитесь, что вы установили службу управления, при добавлении роли IIS.)

Наконец откройте порт 8172 для TCP. Это порт, который используется средством веб-развертывания.

Теперь все готово для развертывания проекта Visual Studio с компьютера разработки на сервер. В обозревателе решений щелкните правой кнопкой мыши решение и нажмите кнопку **публикации**.

Дополнительные сведения о веб-развертывания, см. в разделе [веб-развертывания содержимого карты для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и см. в разделе, что каждый из них сообщения SignalR от другого. (Конечно, в рабочей среде, два сервера бы сидеть балансировщиком нагрузки.)

![](scaleout-with-sql-server/_static/image7.png)

После запуска приложения, вы увидите, что SignalR автоматически создала таблиц в базе данных:

![](scaleout-with-sql-server/_static/image8.png)

SignalR управляет таблицами. До тех пор, пока развертывается приложение, не удалять строки, изменить таблицу и т. д.
