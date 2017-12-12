---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "Жизненный цикл приложения ASP.NET MVC 5 | Документы Microsoft"
author: cephalin
description: "Загрузите документ PDF, что диаграммы жизненного цикла приложения ASP.NET MVC 5. В этом документе жизненного цикла показано высокоуровневое представление жизненного цикла MVC..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Жизненный цикл приложения ASP.NET MVC 5
====================
по [ло Cephas](https://github.com/cephalin)

[Скачать PDF-документ](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Здесь вы можете загрузить PDF-документ, диаграммы жизненного цикла каждого приложения ASP.NET MVC 5, получать HTTP запроса на отправку HTTP-ответ обратно клиенту. Она предназначена, как средство обучения для тех, кто не знакомы с ASP.NET MVC и справочником для тех, которым необходимо детализировать определенные аспекты приложения. PDF-документ имеет следующие особенности:

- Соответствующие [HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) этапов поможет вам понять, где MVC интегрируется в [жизненного цикла приложения ASP.NET](https://msdn.microsoft.com/en-us/library/bb470252.aspx).
- Высокоуровневое представление о жизненном цикле приложений MVC, где можно понять основные этапы, которые каждое приложение MVC передает в конвейер обработки запросов.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Подробное представление, показывающий Детализирует вниз к подробному конвейер обработки запросов. Вы можете сравнить высокоуровневое представление и подробное представление, чтобы увидеть, как собираются сведения о жизненных циклах на несколько этапов. [Скачать PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) для просмотра его увеличения.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Расположение и назначение всех переопределяемых методов на [контроллера](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx) объект в конвейер обработки запросов. Можно или нет необходимости переопределять любые один метод, но важно понимать их роли в жизненном цикле приложения, что на стадии соответствующие жизненного цикла для эффекта, которую вы намерены можно писать код.
- Схемы из дутого вверх, показывающий способ вызова каждого типа фильтра (проверки подлинности, авторизации, действий и результатов).
- Свяжите полезные статьи или блога из каждой точки интерес в представлении «Подробности».


## <a name="next-steps"></a>Дальнейшие действия

Этот документ соответствует требованиям? Мы ценим ваши отзывы. Если у вас есть какие-либо вопросы о жизненном цикле ASP.NET MVC в вашем приложении [Stackoverflow](http://stackoverflow.com/help) и [форумы по ASP.NET MVC](https://forums.asp.net/1146.aspx) — отличное место для запроса. Выполните [мне](https://twitter.com/Cephas_MSFT) в twitter, поэтому можно получить обновления на Мои последние учебники.
