---
uid: signalr/overview/performance/scaleout-with-redis
title: "SignalR горизонтального масштабирования с помощью Redis | Документы Microsoft"
author: MikeWasson
description: "Версии программного обеспечения используется в этом разделе Visual Studio 2013 .NET 4.5 SignalR предыдущие версии версии 2 в этом разделе сведения о предыдущих версиях..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 965c32a4e2f2c9c4bd457d0c13ae99c1378c22c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis"></a>SignalR горизонтального масштабирования с Redis
====================
по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версии 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
> 
> Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


В этом учебнике используется [Redis](http://redis.io/) для распределения сообщений между SignalR приложения, которое развертывается на два отдельных экземпляра IIS.

Redis представляет хранилищу ключей и значений в памяти. Оно также поддерживает системы обмена сообщениями с моделью, публикации или подписки. На задней стороне SignalR Redis использует функцию pub/sub для пересылки сообщений на другие серверы.

![](scaleout-with-redis/_static/image1.png)

В этом учебнике используется три сервера:

- Два сервера под управлением Windows, который будет использоваться для развертывания приложения SignalR.
- Один сервер под управлением Linux, который будет использоваться для выполнения Redis. Снимки экрана, в этом учебнике я использовал Ubuntu 12.04 TLS.

При отсутствии трех физических серверов для использования, можно создать виртуальные машины в Hyper-V. Другой вариант — создать виртуальные машины в Azure.

Несмотря на то, что в этом учебнике используется официальный реализацию Redis, имеется также [Windows порт из Redis](https://github.com/MSOpenTech/redis) из MSOpenTech. Установка и настройка различаются, а в противном случае действия одинаковы.

> [!NOTE] 
> 
> SignalR горизонтального масштабирования с помощью Redis не поддерживает кластеры Redis.


## <a name="overview"></a>Обзор

Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.

1. Установка Redis и запуск сервера Redis.
2. Добавьте эти пакеты NuGet для приложения: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Создайте приложение SignalR.
4. Добавьте следующий код в файле Startup.cs для настройки на задней стороне: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu на узле Hyper-V

С помощью Windows Hyper-V, можно легко создать Виртуальной машине Ubuntu на Windows Server.

Загрузите ISO-образ Ubuntu от [http://www.ubuntu.com](http://www.ubuntu.com/).

В Hyper-V Добавление новой виртуальной Машины. В **подключить виртуальный жесткий диск** шаг, выберите **создать виртуальный жесткий диск**.

![](scaleout-with-redis/_static/image2.png)

В **параметры установки** шаг, выберите **файл образа (.iso)**, нажмите кнопку **Обзор**и перейдите к установке Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Установить Redis

Следуйте указаниям в [http://redis.io/download](http://redis.io/download) загрузить и построить Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Это создает двоичные файлы Redis `src` каталога.

По умолчанию Redis не требует пароля. Чтобы указать пароль, изменить `redis.conf` файл, расположенный в корневом каталоге исходного кода. (Сделать резервную копию файла, перед внесением изменений!) Добавьте следующую директиву для `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Теперь запустите сервер Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Открытый порт 6379, который Redis порта по умолчанию прослушивает. (Можно изменить номер порта в файле конфигурации).

## <a name="create-the-signalr-application"></a>Создание приложения SignalR

Создайте приложение SignalR, выполните одно из этих учебников.

- [Приступая к работе с SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Приступая к работе с SignalR 2.0 и MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Затем мы изменим приложения разговора для поддержки горизонтального масштабирования с помощью Redis. Сначала добавьте пакет SignalR.Redis NuGet в проект. В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Затем откройте файл файле Startup.cs. Добавьте следующий код в **конфигурации** метод:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- «сервер» является имя сервера, на котором выполняется Redis.
- *порт* — номер порта
- «пароль» — это пароль, который определен в файле redis.conf.
- «Имя_приложения» — это любая строка. SignalR создает Redis pub/sub канал с таким именем.

Пример:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Развертывание и запуск приложения

Подготовьте экземпляры сервера Windows для развертывания приложения SignalR.

Добавьте роль IIS. Включить функции «Разработка приложений», включая протокол WebSocket.

![](scaleout-with-redis/_static/image5.png)

Также можно включить службу управления (в области «Средства управления»).

![](scaleout-with-redis/_static/image6.png)

**Установка веб-развертывание 3.0.** При выполнении диспетчера служб IIS, система предложит установить веб-платформы Майкрософт или вы можете [загрузки intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). В установщике платформы поиск веб-развертывания и установка Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Проверьте, что запущена служба управления. В противном случае запустите службу. (Если вы не видите управления веб-службы в списке служб Windows, убедитесь, что установлена служба управления, при добавлении роли IIS.)

Управление веб-службы по умолчанию прослушивает TCP-порт 8172. В брандмауэре Windows создайте новое правило входящего трафика, разрешающее трафик TCP через порт 8172. Дополнительные сведения см. в разделе [Настройка правил брандмауэра](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx). (При размещении виртуальных машин в Azure, это можно сделать непосредственно на портале Azure. В разделе [Настройка конечных точек для виртуальной машины](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)

Теперь вы готовы к развертыванию проекта Visual Studio с компьютера разработки на сервер. В обозревателе решений щелкните правой кнопкой мыши решение и выберите **публикации**.

Подробную документацию по веб-развертывания см. в разделе [Web Карта содержимого развертывания для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и в разделе, чтобы они могли получать сообщений SignalR от другого. (Конечно, в рабочей среде, два сервера будет разместить в подсистему балансировки нагрузки.)

![](scaleout-with-redis/_static/image8.png)

Если вы хотите, чтобы сообщения, отправленные Redis, можно использовать **redis cli** клиента, которая устанавливается вместе с Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
