---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: Практические Работы с URL-адресами в маршрутизации ASP.NET? | Документы Майкрософт
author: rick-anderson
description: В этом видео Крис Пелз показано, как указать URL-адреса для веб-сайта, использующего маршрутизации ASP.NET. Во-первых создается веб-сайта и маршрутизация определяется в ГК...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: 14af9797d916dbda307ce158f50da2ad0bac6e9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364249"
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a>Практические Работы с URL-адресами в маршрутизации ASP.NET?
====================
по [Крис Пелз](https://twitter.com/chrispels)

В этом видео Крис Пелз показано, как указать URL-адреса для веб-сайта, использующего маршрутизации ASP.NET. Во-первых создается веб-сайта и маршрутизация определяется в глобальный класс приложения (.asax). Затем создается пример веб-страницы и URL-адрес, на основе определенному маршруту добавляется на страницу, используя стандартный «жестко запрограммированный» подход, например, «~/Stats/Visitors». Затем другую ссылку добавляется на страницу, который динамически создает же URL-адрес в разметке, с помощью метода RouteValue, который принимает имя маршрута и параметры. Же URL-адрес затем реализуется с помощью кода, а не разметку непосредственно в страницу. Затем изменяются исходный маршрут и расположение физической страницы, в результате чего жестко запрограммированные ссылки больше не работает, тогда как оба динамически создаются работы ссылок должным образом. Наконец затем рассматривается значение динамически генерируемые ссылки обеспечивают доступ.

[&#9654;Просмотрите видео (20 минут)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [Назад](how-do-i-use-routing-with-aspnet-web-forms.md)
