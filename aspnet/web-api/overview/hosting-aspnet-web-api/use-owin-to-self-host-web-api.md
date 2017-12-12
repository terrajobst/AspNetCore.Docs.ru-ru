---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "Используйте OWIN для самостоятельного размещения ASP.NET Web API 2 | Документы Microsoft"
author: rick-anderson
description: "Этот учебник показывает, как для размещения в консольном приложении, с помощью OWIN для самостоятельного размещения платформа веб-API ASP.NET Web API. Открыть веб-интерфейс для .NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Используйте OWIN для самостоятельного размещения ASP.NET Web API 2
====================
по [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Этот учебник показывает, как для размещения в консольном приложении, с помощью OWIN для самостоятельного размещения платформа веб-API ASP.NET Web API.
> 
> [Открыть веб-интерфейс .NET](http://owin.org) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений. OWIN разрывает связь веб-приложения с сервера, благодаря чему идеально подходит для размещения веб-приложения в свой собственный процесс, за пределами служб IIS на собственном сервере OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (также работает с Visual Studio 2012)
> - Веб-API 2


> [!NOTE]
> Можно найти полный исходный код для этого учебника в [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Создайте консольное приложение

На **файл** меню, нажмите кнопку **New**, нажмите кнопку **проекта**. Из **установленные шаблоны**, в Visual C#, нажмите кнопку **Windows** и нажмите кнопку **консольное приложение**. Назовите проект «OwinSelfhostSample» и нажмите кнопку **ОК**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Добавление веб-API и пакеты OWIN

Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, нажмите кнопку **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Будет выполнена установка пакета selfhost WebAPI OWIN и все необходимые пакеты OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Настройка веб-API для резидентного размещения

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класса** для добавления нового класса. Присвойте классу имя `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Добавить контроллер Web API

Добавьте класс контроллера веб-API. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класса** для добавления нового класса. Присвойте классу имя `ValuesController`.

Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Запустить узел OWIN и выполните запрос с помощью HttpClient

Замените весь код шаблона в файле Program.cs следующим кодом:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Запуск приложения

Чтобы запустить приложение, нажмите клавишу F5 в Visual Studio. Этот вывод должен выглядеть так:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

[Общие сведения о проекте Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Размещения веб-API ASP.NET в рабочей роли Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
