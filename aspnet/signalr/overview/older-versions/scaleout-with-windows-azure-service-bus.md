---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Масштабирование SignalR с помощью Azure Service Bus (SignalR 1.x) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 687d3d7787baa69410ee35d651a029c69d28c70b
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287004"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Масштабирование SignalR с помощью Azure Service Bus (SignalR 1.x)
====================
по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

В этом руководстве вы развернете приложении SignalR для веб-роли Windows Azure, использование объединительной платы служебной шины для распределения сообщений для каждого экземпляра роли.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Предварительные требования

- Учетная запись Windows Azure.
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

На задней стороне шины службы также совместима с [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), версии 1.1. Тем не менее не совместима с версией 1.0 Service Bus for Windows Server.

## <a name="pricing"></a>Цены

На задней стороне служебная шина использует разделы для отправки сообщений. Сведения о последних ценах, см. в разделе [служебной шины](https://azure.microsoft.com/pricing/details/service-bus/). Во время написания данной статьи вы можете отправлять 1 000 000 сообщений в месяц для меньше, чем 1 долл. США. Задняя панель отправляет сообщение служебной шины для каждого вызова метода концентратора SignalR. Кроме того, существуют также некоторые сообщения управления для подключения, отключения, присоединяемый или создание групп и т. д. В большинстве приложений большая часть трафика сообщений будет вызовы методов концентратора.

## <a name="overview"></a>Обзор

Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.

1. Создать новое пространство имен служебной шины с помощью портала Windows Azure.
2. Добавьте следующие пакеты NuGet приложения: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Создайте приложение SignalR.
4. Добавьте следующий код в Global.asax, чтобы настроить на задней стороне: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Для каждого приложения выберите другое значение для «YourAppName». Не используйте то же значение в нескольких приложениях.

## <a name="create-the-azure-services"></a>Создание служб Azure

Создание облачной службы, как описано в разделе [как создать и развернуть облачную службу](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Выполните действия, описанные в разделе «как: Создание облачной службы с помощью функции быстрого создания». В этом руководстве вы не обязательно должны передать сертификат.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Создайте новое пространство имен служебной шины, как описано в [как для использования разделов и подписок служебной шины](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Выполните действия, описанные в разделе «Создание имен службы».

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Убедитесь в том, что выберите тот же регион для облачной службы и пространство имен служебной шины.


## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

Запустите Visual Studio. Из **файл** меню, щелкните **новый проект**.

В **новый проект** диалоговом окне разверните **Visual C#**. В разделе **установленные шаблоны**выберите **Cloud** , а затем выберите **облачной службы Windows Azure**. Оставьте по умолчанию .NET Framework 4.5. Присвойте имя приложению ChatService, а затем нажмите кнопку **ОК**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

В **новая облачная служба Windows Azure** диалоговое окно, выберите веб-роль ASP.NET MVC 4. Нажмите кнопку со стрелкой вправо (**&gt;**) чтобы добавить роль в решение.

Наведите указатель мыши новую роль, поэтому отображается значок с изображением карандаша. Щелкните этот значок, чтобы переименовать роль. Имя роли «SignalRChat» и нажмите кнопку **ОК**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

В **создания проекта ASP.NET MVC 4** мастера выберите **веб-приложение**. Нажмите кнопку **ОК**. Мастер проекта создает два проекта:

- ChatService: Этот проект является приложением Windows Azure. Он определяет роли Azure и других параметров конфигурации.
- SignalRChat: Этот проект является проекта ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Создание приложения чата SignalR

Чтобы создать приложение чата, следуйте указаниям в этом руководстве [начало работы с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Используйте NuGet для установки необходимых библиотек. Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В **консоль диспетчера пакетов** окно, введите следующие команды:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Используйте `-ProjectName` параметр для установки пакетов проекта ASP.NET MVC, а не проекта Windows Azure.

## <a name="configure-the-backplane"></a>Настройка задней панели

В файле Global.asax приложения добавьте следующий код:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Теперь необходимо получить строку подключения service bus. На портале Azure выберите пространство имен служебной шины, который вы создали и щелкните значок ключа доступа.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Скопируйте строку подключения в буфер обмена, а затем вставьте его в *connectionString* переменной.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Развертывание в Azure

В обозревателе решений разверните **ролей** папку внутри проекта ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Щелкните правой кнопкой мыши роль SignalRChat и выберите **свойства**. Выберите **конфигурации** вкладки. В разделе **экземпляров** выберите 2. Можно также задать размер виртуальной Машины **очень мелкий**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Сохраните изменения.

В обозревателе решений щелкните правой кнопкой мыши проект ChatService. Нажмите **Публиковать**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Если это ваш первый время публикации в Windows Azure, необходимо загрузить учетные данные. В **публикации** мастера, нажмите кнопку «Вход для загрузки учетных данных». Это предложит Войдите на портал Windows Azure и загрузите файл параметров публикации.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Нажмите кнопку **импорта** и выберите загруженный файл параметров публикации.

Нажмите кнопку **Далее**. В **параметры публикации** диалогового окна в разделе **облачной службы**, выберите облачную службу, созданную ранее.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Нажмите кнопку **Опубликовать**. Может занять несколько минут, чтобы развернуть приложение и запустить виртуальные машины.

Теперь при запуске приложения чата, экземпляры ролей, взаимодействуют через служебную шину Azure, используя раздел служебной шины. Тема — это очередь сообщений, которая позволяет нескольким подписчикам.

Задняя панель автоматически создает раздела и подписки. Чтобы увидеть подписки и действие сообщения, откройте портал Azure, выберите пространство имен служебной шины и щелкните «Разделы».

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Берет на себя занять несколько минут для действия сообщения для отображения на панели мониторинга.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR управляет временем существования раздела. До тех пор, пока развертывается приложение, не следует пытаться вручную удалить разделы или изменить параметры в разделе.
