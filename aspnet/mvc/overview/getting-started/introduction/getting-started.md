---
uid: mvc/overview/getting-started/introduction/getting-started
title: "Приступая к работе с ASP.NET MVC 5 | Документы Microsoft"
author: Rick-Anderson
description: "Примечание: Обновленную версию этого учебника доступен с помощью Visual Studio 2015. Новое в этом учебнике используется ASP.NET Core MVC 6, которая предоставляет много improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>Начало работы с ASP.NET MVC 5
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 Этот учебник поможет узнать основы создания приложений веб-ASP.NET MVC 5 с помощью [2017 г. Visual Studio](https://www.visualstudio.com/). Окончательный источника для учебника, расположенных на [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 Это руководство было написано с [Скотт Гатри](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , и [Рик Андерсон](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 Требуется учетная запись Azure для развертывания этого приложения в Azure:
 
 - Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.
 - Вы можете [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.


## <a name="getting-started"></a>Начало работы

Начните с установки и запуска [2017 г. Visual Studio](https://www.visualstudio.com/).

Visual Studio — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений. В Visual Studio имеется список вдоль нижней отображаются различные параметры, доступные для вас. Кроме того, имеется меню, которое предоставляет еще один способ выполнения задач в Интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выбрать **файл** &gt; **новый проект**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>Создание первого приложения

Нажмите кнопку **новый проект**, выберите Visual C# слева, затем **Web** , а затем выберите **веб-приложения ASP.NET (.NET Framework)**. Назовите проект «MvcMovie» и нажмите кнопку **ОК**.

![](getting-started/_static/image2.png)

В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC** и нажмите кнопку **ОК**.

![](getting-started/_static/image3.png)

Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий! Это простое «Hello World!» проекта, а вот хорошая можно запустить приложение.

![](getting-started/_static/image4.png)

Нажмите F5 для запуска отладки. F5 приводит Visual Studio для запуска [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) и запуска веб-приложения. Затем Visual Studio запускает браузер и открывает домашнюю страницу приложения. Обратите внимание, что в адресной строке браузера говорит `localhost:port#` , а не то, как `example.com`. Причина этого заключается в `localhost` всегда указывает на локальном компьютере, работающая в этом случае просто создано приложение. При запуске веб-проекта Visual Studio для веб-сервера используется случайный порт. На рисунке ниже номер порта — 1234. При запуске приложения вы увидите другой номер порта.

![](getting-started/_static/image5.png)

Сразу после распаковки этот шаблон по умолчанию дает дома, контактов и о страницы. На рисунке выше не показывает **Главная**, **о** и **контакт** ссылки. В зависимости от размера окна браузера может потребоваться щелкните значок навигации, чтобы найти по следующим ссылкам.

![](getting-started/_static/image6.png)  

Приложение также поддерживает для регистрации и входа. Следующий шаг — изменить работу этого приложения и немного узнать о ASP.NET MVC. Закройте приложение ASP.NET MVC и изменим некоторый код.

Список текущего учебники см. в разделе [MVC рекомендуется статьи](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Это приложение, работающее на платформе Azure см.

Вы действительно хотите см. по завершении узел, работающий как динамические веб-приложения? Можно развернуть полную версию приложения для учетной записи Azure, просто нажав кнопку ниже.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Требуется учетная запись Azure для развертывания этого решения в Azure. Если у вас еще нет учетной записи, у вас есть следующие параметры:

- [Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.
- [Активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.

>[!div class="step-by-step"]
[Вперед](adding-a-controller.md)
