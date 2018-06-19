---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Инструкции:] Кэширование страницы ASP.NET на основе данных пользовательского элемента управления | Документы Microsoft'
author: rick-anderson
description: В этой видео пиксел Крис показывает способ управления критерии для кэширования страницы ASP.NET на основе пользовательских данных. Пример страницы создается и затем O...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528133"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a>[Инструкции:] Кэширование страницы ASP.NET на основе данных пользовательского элемента управления
====================
по [Крис пиксел](https://twitter.com/chrispels)

В этой видео пиксел Крис показывает способ управления критерии для кэширования страницы ASP.NET на основе пользовательских данных. Пример страницы создается и используется директива OutputCache атрибутом VaryByCustom, который содержит пользовательское значение. Затем метод GetVaryCustomByString() переопределяется в модуль global.asax, который обеспечивает обработку настраиваемого атрибута. В этом методе возвращается строка, однозначно идентифицирующий кэшированная версия страницы. Наконец есть обсуждение как кэширование, используя пользовательское значение может использоваться несколькими способами для веб-сайта.

[&#9654; Посмотрите видео (12 минут)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
