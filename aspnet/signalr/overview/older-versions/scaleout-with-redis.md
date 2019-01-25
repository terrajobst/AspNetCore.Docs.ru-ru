---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Масштабирование SignalR с помощью Redis (SignalR 1.x) | Документация Майкрософт
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 74294bd04d5649f2ec54e58adb744f5e30525162
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836419"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>Масштабирование SignalR с помощью Redis (SignalR 1.x)
====================
по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

В этом руководстве вы воспользуетесь [Redis](http://redis.io/) чтобы распределить сообщения в приложении SignalR, которое развертывается на два отдельных экземпляра IIS.

Redis — это хранилище ключ значение в памяти. Она также поддерживает систему обмена сообщениями с помощью модели публикации и подписки. На задней стороне SignalR Redis используется функция pub/sub для перенаправления сообщений к другим серверам.

![](scaleout-with-redis/_static/image1.png)

В этом учебнике будет использоваться на трех серверах:

- Два сервера под управлением Windows, который будет использоваться для развертывания приложения SignalR.
- Один сервер под управлением Linux, который будет использоваться для выполнения Redis. Снимки экрана в этом руководстве я использовал Ubuntu 12.04 TLS.

Если у вас нет трех физических серверов для использования, можно создать виртуальные машины в Hyper-V. Другой вариант — для создания виртуальных машин в Azure.

Несмотря на то, что в этом учебнике используется официальный реализация Redis, имеется также [Windows порт из Redis](https://github.com/MSOpenTech/redis) из MSOpenTech. Установка и настройка различаются, но в противном случае действия одинаковы.

> [!NOTE] 
> 
> Масштабирование SignalR с помощью Redis не поддерживает кластеры Redis.


## <a name="overview"></a>Обзор

Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.

1. Установите Redis и запустить сервер Redis.
2. Добавьте следующие пакеты NuGet приложения: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Создайте приложение SignalR.
4. Добавьте следующий код в Global.asax, чтобы настроить на задней стороне: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu в Hyper-V

С помощью Windows Hyper-V, можно легко создать виртуальной Машины Ubuntu в Windows Server.

Скачайте ISO-образ Ubuntu из [ http://www.ubuntu.com ](http://www.ubuntu.com/).

В Hyper-V добавьте новую виртуальную Машину. В **подключить виртуальный жесткий диск** выберите **создать виртуальный жесткий диск**.

![](scaleout-with-redis/_static/image2.png)

В **параметры установки** выберите **файл образа (.iso)**, нажмите кнопку **Обзор**и перейдите к Ubuntu установочный ISO-образ.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Установите Redis

Следуйте инструкциям из статьи [ http://redis.io/download ](http://redis.io/download) для скачивания и сборки Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Это создает двоичные файлы Redis `src` каталога.

По умолчанию Redis не требует пароля. Чтобы указать пароль, изменить `redis.conf` файл, который находится в корневой каталог исходного кода. (Перед внесением изменений убедитесь в резервную копию файла!) Добавьте следующую директиву для `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Теперь запустите сервер Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Открытие порта 6379, который является портом по умолчанию, Redis прослушивает. (Можно изменить номер порта в файле конфигурации.)

## <a name="create-the-signalr-application"></a>Создание приложения SignalR

Создайте приложение SignalR, выполнив одно из следующих руководств:

- [Начало работы с SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Начало работы с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Затем мы изменим приложение чата для поддержки горизонтального масштабирования с помощью Redis. Во-первых добавьте пакет SignalR.Redis NuGet в проект. В Visual Studio из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Затем откройте файл Global.asax. Добавьте следующий код, чтобы **приложения\_запустить** метод:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- «server» — имя сервера, на котором выполняется Redis.
- *порт* — номер порта
- «password» — это пароль, который определен в файле redis.conf.
- «AppName» — любая строка. SignalR создает канал Redis pub/sub с таким именем.

Пример:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Развертывание и запуск приложения

Подготовка экземпляров Windows Server для развертывания приложения SignalR.

Добавьте роль IIS. Включить функции «Разработка приложений», включая протокол WebSocket.

![](scaleout-with-redis/_static/image5.png)

Также службы управления (см. в разделе «Средства управления»).

![](scaleout-with-redis/_static/image6.png)

**Установить Web Deploy 3.0.** Если запустить диспетчер служб IIS, он предложит вам установить веб-платформы Майкрософт или вы можете [загрузить intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). В установщике платформы найдите веб-развертывания и установить Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Убедитесь, что веб-службы управления. В противном случае запустите службу. (Если вы не видите веб-службы управления в списке служб Windows, убедитесь, что вы установили службу управления, при добавлении роли IIS.)

Веб-служба управления по умолчанию прослушивает TCP-порт 8172. В брандмауэре Windows создайте новое входящее правило разрешает TCP-трафик через порт 8172. Дополнительные сведения см. в разделе [Настройка правил брандмауэра](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Если вы размещаете виртуальные машины в Azure, вы это можно сделать непосредственно на портале Azure. См. в разделе [Настройка конечных точек для виртуальной машины](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Теперь все готово для развертывания проекта Visual Studio с компьютера разработки на сервер. В обозревателе решений щелкните правой кнопкой мыши решение и нажмите кнопку **публикации**.

Дополнительные сведения о веб-развертывания, см. в разделе [веб-развертывания содержимого карты для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и см. в разделе, что каждый из них сообщения SignalR от другого. (Конечно, в рабочей среде, два сервера бы сидеть балансировщиком нагрузки.)

![](scaleout-with-redis/_static/image8.png)

Если вы хотите просмотреть сообщения, отправляемые к Redis, можно использовать **redis-cli** клиент, который устанавливается вместе с Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
