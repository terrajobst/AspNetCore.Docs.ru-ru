---
uid: mvc/overview/getting-started/introduction/getting-started
title: Приступая к работе с ASP.NET MVC 5 | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь с помощью Visual Studio 2015. Новое учебное использует ASP.NET Core MVC 6, которая предоставляет новых преимуществах...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0585e3a841aef72a17d966041029ff7be129a2b3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402127"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Начало работы с ASP.NET MVC 5
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Этот учебник поможет основы создания приложения web ASP.NET MVC 5 с помощью [Visual Studio 2017](https://www.visualstudio.com/). Окончательный источника для работы с руководством, расположенных на [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Это руководство было написано с [Скотт Гатри](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [(Scott hanselman)](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , и [Рик Андерсон](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Вам потребуется учетная запись Azure, чтобы развернуть это приложение в Azure:

 - Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать для опробования платных служб Azure и даже в том случае, если они используются, вы сохраняете учетную запись и использовать бесплатные службы Azure.
 - Вы можете [активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ваша подписка MSDN предоставляет вам кредиты каждый месяц, который можно использовать для оплаты служб Azure.


## <a name="getting-started"></a>Начало работы

Начните с установки и запуска [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio является интегрированной среды разработки или интегрированной среды разработки. Так же, как использовать Microsoft Word для записи документов, вы используете интегрированную среду разработки для создания приложений. В Visual Studio имеется список внизу отображаются различные параметры, доступные для вас. Имеется также меню, которое предоставляет еще один способ выполнения задач в интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выберите **файл** &gt; **новый проект**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Создание первого приложения

Нажмите кнопку **новый проект**, затем выберите Visual C# в левой части, а затем **Web** , а затем выберите **веб-приложение ASP.NET (.NET Framework)**. Назовите проект «MvcMovie» и нажмите кнопку **ОК**.

![](getting-started/_static/image2.png)

В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC** и нажмите кнопку **ОК**.

![](getting-started/_static/image3.png)

Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому у вас есть рабочее приложение прямо сейчас не выполняя никаких действий! Это связано с простых «Hello World!» проекта, а вот отлично подходит для запуска приложения.

![](getting-started/_static/image4.png)

Нажмите F5, чтобы начать отладку. F5 приводит к Visual Studio запускать [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) и запуска веб-приложения. Затем Visual Studio запустит браузер и откроется домашняя страница приложения. Обратите внимание, что говорит в адресной строке браузера `localhost:port#` и не что-либо типа `example.com`. Это потому, что `localhost` всегда указывает на локальном компьютере, который выполняется, в этом случае приложение, вы только что выполнили. При запуске веб-проекта Visual Studio для веб-сервера используется случайный порт. На рисунке ниже номер порта — 1234. При запуске приложения вы увидите другой номер порта.

![](getting-started/_static/image5.png)

Готовые этот шаблон по умолчанию позволяет дома, контактов и о страницы. На рисунке выше не показывает **Главная**, **о** и **контакт** ссылки. В зависимости от размера окна браузера может потребоваться щелкнуть значок навигации, чтобы найти по следующим ссылкам.

![](getting-started/_static/image6.png)  

Приложение также предоставляет поддержку для регистрации и входа. Следующий шаг — изменить работу этого приложения и немного поговорим об ASP.NET MVC. Закройте приложение ASP.NET MVC и изменим код.

Список действующие руководства, см. в разделе [MVC рекомендуется статьи](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Это приложение работает в Azure см. в разделе

Вы действительно хотите см. по завершении сайт, запущенный в качестве активного веб-приложения? Полную версию приложения можно развернуть учетную запись Azure, просто нажав кнопку ниже.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Требуется учетная запись Azure для развертывания этого решения в Azure. Если у вас еще нет учетной записи, у вас есть следующие параметры:

- [Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать для опробования платных служб Azure и даже в том случае, если они используются, вы сохраняете учетную запись и использовать бесплатные службы Azure.
- [Активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ваша подписка MSDN предоставляет вам кредиты каждый месяц, который можно использовать для оплаты служб Azure.

> [!div class="step-by-step"]
> [Вперед](adding-a-controller.md)
