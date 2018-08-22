---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Жизненный цикл приложения ASP.NET MVC 5 | Документация Майкрософт
author: cephalin
description: Скачайте документ PDF, что диаграммы жизненного цикла приложения ASP.NET MVC 5. В этом документе жизненного цикла показано высокоуровневое представление жизненного цикла MVC...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5523217ae5674ac4e5898a150f9e5f0ae3a70d26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824010"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Жизненный цикл приложения ASP.NET MVC 5
====================
по [Cephas Лин](https://github.com/cephalin)

[Скачать PDF-документ](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Здесь вы можете скачать PDF-документ, диаграммы, жизненного цикла каждого приложения ASP.NET MVC 5, получение HTTP запроса на отправку HTTP-ответ обратно клиенту. Она предназначена, так как средство обучения для тех, кто еще не работали с ASP.NET MVC, а также как ссылку для тех, кому требуется вникать в определенных аспектов приложения. PDF-документ имеет следующие особенности:

- Соответствующие [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) этапы, которые помогут вам понять, где MVC интегрируется в [жизненный цикл приложения ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Высокоуровневый обзор жизненного цикла приложения MVC, где вы понимаете основные этапы, из которых каждое приложение MVC передает в конвейер обработки запросов.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Подробное представление, показывающий Детализирует вниз подробную информацию о конвейере обработки запросов. Вы можете сравнить высокоуровневое представление и подробное представление, чтобы увидеть, как собираются сведения о жизненных циклах на несколько этапов. [Скачивание PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) для увеличения см. в разделе.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Размещение и цель всеми переопределяемыми методами на [контроллера](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) объекта в конвейере обработки запросов. Может или не иметь необходимости переопределять любые один метод, но важно понимать их роль в жизненном цикле приложения таким образом, вы можете написать код на этапе соответствующие жизненного цикла для эффекта, вы нашли.
- Схемы из дутого вверх, показывающая способ вызова каждого из типов фильтра (проверки подлинности, авторизации, действие и результат).
- Ссылка на полезные статьи или блога из каждого интересующую точку в представлении «Подробности».


## <a name="next-steps"></a>Следующие шаги

Этот документ соответствует требованиям? Мы ценим ваши отзывы. Если вам необходимо узнать о жизненном цикле ASP.NET MVC в приложении, [Stackoverflow](http://stackoverflow.com/help) и [форумах ASP.NET MVC](https://forums.asp.net/1146.aspx) — отличное место для запроса. Выполните [мне](https://twitter.com/Cephas_MSFT) в twitter, поэтому вы можете получать обновления на моем новейшие учебники.
