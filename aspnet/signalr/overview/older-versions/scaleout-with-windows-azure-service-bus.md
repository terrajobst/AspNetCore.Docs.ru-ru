---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: SignalR горизонтального масштабирования с Azure Service Bus (SignalR 1.x) | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036444"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>SignalR горизонтального масштабирования с Azure Service Bus (SignalR 1.x)
====================
по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

В этом учебнике будет развертывание приложения с SignalR для веб-роли Windows Azure, используя на задней стороне Service Bus для распределения сообщений для каждого экземпляра роли.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Предварительные требования

- Учетная запись Windows Azure.
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Задняя панель шины службы также совместим с [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), версия 1.1. Однако он не совместим с версией 1.0 Service Bus for Windows Server.

## <a name="pricing"></a>Цены

На задней стороне Service Bus использует разделы для отправки сообщений. Последнюю информацию, см. [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). На момент написания этой статьи можно отправить 1 000 000 сообщений в месяц для меньше $1. Задняя панель отправляет сообщение служебной шины для каждого вызова метода концентратора SignalR. Существуют также некоторые управляющие сообщения для подключения, отключения, присоединяемый или создание групп и т. д. В большинстве приложений большая часть трафика сообщений будет вызовы методов концентратора.

## <a name="overview"></a>Обзор

Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.

1. Используйте портал Windows Azure для создания нового пространства имен Service Bus.
2. Добавьте эти пакеты NuGet для приложения: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Создайте приложение SignalR.
4. Добавьте следующий код Global.asax для настройки на задней стороне: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Для каждого приложения выберите другое значение для «YourAppName». Не используйте то же значение для нескольких приложений.

## <a name="create-the-azure-services"></a>Создание служб Azure

Создание облачной службы, как описано в [Создание и развертывание облачной службы](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Следуйте указаниям в разделе «как: создать облачную службу с помощью быстрого создания». В этом учебнике необходимо отправить сертификат.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Создайте новое пространство имен Service Bus, как описано в [как для использования службы подписок и разделов шины](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Следуйте указаниям в разделе «Создание службы пространство имен».

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Убедитесь, выбираемых в том же регионе для облачной службы и пространство имен служебной шины.


## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

Запустите Visual Studio. Из **файл** меню, нажмите кнопку **новый проект**.

В **новый проект** диалогового окна разверните **Visual C#**. В разделе **установленные шаблоны**выберите **облака** , а затем выберите **облачных служб Windows Azure**. Оставьте значение по умолчанию .NET Framework 4.5. Имя приложения ChatService и нажмите кнопку **ОК**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

В **новой облачной службы Windows Azure** диалоговое окно, выберите веб-роль ASP.NET MVC 4. Нажмите кнопку со стрелкой вправо (**&gt;**) чтобы добавить роль в решение.

Наведите указатель мыши новой роли, поэтому отображается значок карандаша. Щелкните этот значок, чтобы переименовать роль. Имя роли «SignalRChat» и нажмите кнопку **ОК**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

В **нового проекта ASP.NET MVC 4** мастера выберите **веб-приложение**. Нажмите кнопку **ОК**. Мастер проекта создает два проекта:

- ChatService: В этот проект является приложением Windows Azure. Он определяет роли Azure и других параметров конфигурации.
- SignalRChat: Этот проект является проект ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Создание приложения разговора SignalR

Чтобы создать приложения разговора, выполните шаги в учебнике [Приступая к работе с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Установите необходимые библиотеки с помощью NuGet. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В **консоль диспетчера пакетов** окно, введите следующие команды:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Используйте `-ProjectName` параметр для установки пакетов проекта ASP.NET MVC, а не в проекте Windows Azure.

## <a name="configure-the-backplane"></a>Настройка объединительной плате

В файле Global.asax приложения добавьте следующий код:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Теперь вам нужно получить строки подключения service bus. На портале Azure выберите пространство имен служебной шины, созданный и щелкните значок ключа доступа.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Скопируйте строку подключения в буфер обмена, а затем вставьте его в *connectionString* переменной.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Развертывание в Azure

В обозревателе решений откройте **ролей** папку внутри проекта ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Щелкните правой кнопкой мыши роль SignalRChat и выберите **свойства**. Выберите **конфигурации** вкладки. В разделе **экземпляров** выберите 2. Можно также задать размер виртуальной Машины **чрезвычайно малая**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Сохраните изменения.

В обозревателе решений щелкните правой кнопкой мыши проект ChatService. Нажмите **Публиковать**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Если это ваш первый время публикации в Windows Azure, необходимо загрузить учетные данные. В **публикации** мастера, нажмите кнопку «Вход для загрузки учетных данных». Появится запрос для входа на портал Windows Azure и загрузите файл параметров публикации.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Нажмите кнопку **импорта** и выберите загруженный файл параметров публикации.

Нажмите кнопку **Далее**. В **параметры публикации** диалогового окна в разделе **облачной службы**, выберите ранее созданную облачную службу.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Нажмите кнопку **Опубликовать**. Он может занять несколько минут, чтобы развернуть приложение и запустить виртуальные машины.

Теперь при запуске приложения разговора экземпляры роли, взаимодействуют через Azure Service Bus с помощью раздела Service Bus. Раздел является очередью сообщений, которая позволяет нескольким подписчикам.

Объединительной плате автоматически создает раздел и подписки. Для просмотра подписок и действие сообщения, откройте портал Azure, выберите пространство имен служебной шины и выберите команду «Разделы».

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Он может занять несколько минут, действие сообщения, отображаются в панели мониторинга.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR управляет временем существования раздела. При условии, что приложение развертывается, не пытайтесь вручную удалить разделы или измените параметры в разделе.
