---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[Инструкции:] Чтобы заменить HTML на странице ASP.NET используйте свойство Reponse.Filter | Документы Microsoft"
author: rick-anderson
description: "В этот видеоролик пиксел Крис показывает, как использовать свойство Reponse.Filter перехватывать и изменять, отправляемых на страницу HTML. Во-первых пример страницы создается w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Инструкции:] Свойство Reponse.Filter позволяет заменить HTML на странице ASP.NET
====================
по [Крис пиксел](https://twitter.com/chrispels)

В этот видеоролик пиксел Крис показывает, как использовать свойство Reponse.Filter перехватывать и изменять, отправляемых на страницу HTML. Во-первых пример страницы создается простой текст. Затем создается пользовательский класс потока, служащий поток замены для текущего потока, отправляемых в браузер пользователя. В этом классе пользовательского потока содержимого страницы извлекаются из потока, изменить, а затем записывается в поток ответа. В этот настраиваемый класс потока метода записи настроено таким образом, чтобы заменить HTML в базовый поток ответа, тем самым изменение отправки в браузер пользователя. Наконец, новый класс stream назначается свойство Response.Filter на странице\_событию загрузки, тем самым предоставляя механизм для изменения содержимого страницы.

[&#9654; Посмотрите видео (13 мин.)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
