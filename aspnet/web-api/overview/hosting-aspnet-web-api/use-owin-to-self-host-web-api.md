---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Использование OWIN для резидентного размещения веб-API 2 ASP.NET | Документация Майкрософт
author: rick-anderson
description: Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API. Откройте веб-интерфейс для .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 06fd13fe9b12d172d615ae76a71d246a89f5386d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910490"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Использование OWIN для резидентного размещения веб-API 2 ASP.NET
====================
по [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API.
>
> [Открыть веб-интерфейс .NET](http://owin.org) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений. OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами служб IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (также работает с Visual Studio 2012)
> - Веб-API 2


> [!NOTE]
> Полный исходный код для выполнения инструкций этого руководства можно найти [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Создайте консольное приложение

На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**. Из **установленные шаблоны**, в разделе Visual C#, щелкните **Windows** и нажмите кнопку **консольное приложение**. Присвойте проекту имя «OwinSelfhostSample» и нажмите кнопку **ОК**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Добавьте веб-API и пакеты OWIN

Из **средства** меню, щелкните **диспетчер пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Это установит пакет selfhost OWIN веб-API и все необходимые пакеты OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Настройка веб-API для резидентного размещения

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса. Присвойте классу имя `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Добавить контроллер веб-API

Добавьте класс контроллера веб-API. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса. Присвойте классу имя `ValuesController`.

Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Запустите хоста OWIN и сделать запрос с помощью HttpClient

Замените весь код шаблона в файле Program.cs следующим кодом:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Запуск приложения

Чтобы запустить приложение, нажмите клавишу F5 в Visual Studio. Этот вывод должен выглядеть так:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

[Обзор проекта Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Размещение веб-API ASP.NET в рабочей роли Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
