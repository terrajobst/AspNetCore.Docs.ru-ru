---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: "SignalR горизонтального масштабирования с SQL Server (SignalR 1.x) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>SignalR горизонтального масштабирования с SQL Server (SignalR 1.x)
====================
по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

В этом учебнике будет использоваться SQL Server для распределения сообщений между SignalR приложения, которое развертывается в двух разных экземплярах служб IIS. Этот учебник можно также запустить на одном тестовом компьютере, но чтобы получить результат, необходимо развернуть приложение SignalR на несколько серверов. Также необходимо установить SQL Server на одном из серверов или на отдельном выделенном сервере. Другой вариант — для работы с учебником, с использованием виртуальных машин в Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Предварительные требования

Microsoft SQL Server 2005 или более поздней версии. Задняя панель поддерживает выпуски настольных компьютеров и серверов SQL Server. Он не поддерживает SQL Server Compact Edition или базы данных SQL Azure. (Если приложение размещено в Azure, рекомендуется на задней стороне служебной шины вместо.)

## <a name="overview"></a>Обзор

Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.

1. Создайте новую пустую базу данных. Объединительной плате будет создать необходимые таблицы в этой базе данных.
2. Добавьте эти пакеты NuGet для приложения: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Создайте приложение SignalR.
4. Добавьте следующий код Global.asax для настройки на задней стороне: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Настройка базы данных

Решите, будет ли приложение использовать проверку подлинности Windows или проверка подлинности SQL Server для доступа к базе данных. Независимо от того убедитесь, что пользователь базы данных имеет разрешения на вход, создании схем и таблиц.

Создайте новую базу данных для использования объединительной платы. Можно присвоить любое имя базы данных. Не нужно создавать любой таблицы в базе данных. Задняя панель будет создать необходимые таблицы.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Включение компонента Service Broker

Рекомендуется включить компонент Service Broker для базы данных объединительной платы. Компонент Service Broker обеспечивает собственную поддержку для обмена сообщениями и очередей в SQL Server, что позволяет более эффективно получать обновления объединительной плате. (Однако объединительной платы также работает без компонента Service Broker).

Чтобы проверить, включен ли компонент Service Broker, запрос **—\_broker\_включен** столбца в **sys.databases** представления каталога.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Чтобы включить компонент Service Broker, используйте следующий SQL-запрос:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Если взаимоблокировка, убедитесь, что этот запрос нет приложений, подключенных к базе данных.


Если трассировка включена, данные трассировки также будет показано, включен ли компонент Service Broker.

## <a name="create-a-signalr-application"></a>Создание приложения SignalR

Создайте приложение SignalR, выполните одно из этих учебников.

- [Приступая к работе с SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Приступая к работе с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Затем мы изменим приложения разговора для поддержки горизонтального масштабирования с SQL Server. Сначала добавьте пакет SignalR.SqlServer NuGet в проект. В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Затем откройте файл Global.asax. Добавьте следующий код в **приложения\_запустить** метод:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Развертывание и запуск приложения

Подготовьте экземпляры сервера Windows для развертывания приложения SignalR.

Добавьте роль IIS. Включить функции «Разработка приложений», включая протокол WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Также можно включить службу управления (в области «Средства управления»).

![](scaleout-with-sql-server/_static/image5.png)

**Установка веб-развертывание 3.0.** При выполнении диспетчера служб IIS, система предложит установить веб-платформы Майкрософт или вы можете [загрузки intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). В установщике платформы поиск веб-развертывания и установка Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Проверьте, что запущена служба управления. В противном случае запустите службу. (Если вы не видите управления веб-службы в списке служб Windows, убедитесь, что установлена служба управления, при добавлении роли IIS.)

Наконец необходимо откройте порт 8172 протокол TCP. Это порт, который используется средством веб-развертывания.

Теперь вы готовы к развертыванию проекта Visual Studio с компьютера разработки на сервер. В обозревателе решений щелкните правой кнопкой мыши решение и выберите **публикации**.

Подробную документацию по веб-развертывания см. в разделе [Web Карта содержимого развертывания для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и в разделе, чтобы они могли получать сообщений SignalR от другого. (Конечно, в рабочей среде, два сервера будет разместить в подсистему балансировки нагрузки.)

![](scaleout-with-sql-server/_static/image7.png)

После запуска приложения, можно увидеть, что SignalR автоматически создал таблиц в базе данных:

![](scaleout-with-sql-server/_static/image8.png)

SignalR управляет таблиц. При условии, что приложение развертывается, не удалять строки, изменить таблицу и пр.
