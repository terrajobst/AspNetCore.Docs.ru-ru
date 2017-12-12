---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: "Введение в учебник обновление NerdDinner | Документы Microsoft"
author: shanselman
description: "Лучший способ изучения новой платформы является построения каких-то с ним. В этом учебнике представлены пошаговые инструкции для создания приложения с небольшой, но завершенный, с помощью ASP.NE..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 57eedb224e26867c78cc399b89f91b95f722074d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="introducing-the-nerddinner-tutorial"></a>Введение в учебник обновление NerdDinner
====================
по [Скотт Хансельман](https://github.com/shanselman)

[Скачать PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Лучший способ изучения новой платформы является построения каких-то с ним. Этот учебник рассматриваются способы построения небольшой, но завершается приложения с помощью ASP.NET MVC 1 и рассматриваются некоторые основные понятия, стоящий за ней.
> 
> При использовании ASP.NET MVC 3, которому рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.


## <a name="nerddinner-tutorial"></a>Обновление NerdDinner учебника

Лучший способ изучения новой платформы является построения каких-то с ним. Рассматриваются способы построения небольшой, но завершается приложения ASP.NET MVC с помощью этого учебника и рассматриваются некоторые основные понятия, стоящий за ней.

Приложение, которое мы собираемся строить называется «Обновление NerdDinner». Обновление NerdDinner позволяет легко найти и организовать ужинов сети для пользователей:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

Обновление NerdDinner зарегистрированные пользователи могут создавать, изменять и удалять ужинов. Тем самым согласованного набора проверки и бизнес-правила во всем приложении:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Посетители могут использовать карты на основе AJAX для поиска предстоящих ужинов удержания рядом с ними:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Обед щелкнув ведет на страницу сведений где можно узнать дополнительные сведения об этом:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Если они посещение dinner они смогут войти в систему или зарегистрировать на сайте:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Затем необходимо щелкнуть ссылку на основе AJAX RSVP должны посетить события:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Реализация обновление NerdDinner

Мы хотим начать обновление NerdDinner приложения с помощью файла -&gt;команды новый проект в Visual Studio для создания нового проекта ASP.NET MVC. Затем последовательно добавим функциональность и возможности. Попутно мы расскажем:

1. [Создание нового проекта ASP.NET MVC](# "создайте новый проект ASP.NET MVC")
2. [Создание базы данных](# "Создание базы данных")
3. [Как создать модель с бизнес-правило проверок](# "построить модель с бизнес-правило проверок")
4. [Как использовать контроллеры и представления для реализации и сведения о вхождении пользовательского интерфейса](# "использование контроллеров и представлений для реализации пользовательского интерфейса сведения о вхождении")
5. [Как обеспечить CRUD (Создание, чтение, обновление и удаление) данных образуют запись поддержки](# "предоставляют CRUD (Создание, чтение, обновление, удаление) запись данных формы поддерживает")
6. [Способы использования ViewData и реализовать такие классы ViewModel](# "использовать ViewData и реализовать классы ViewModel")
7. [Как повторно использовать пользовательский Интерфейс, с помощью главных страниц и частичными репликами](# "повторного использования пользовательского интерфейса с помощью главных страниц и частичными репликами")
8. [Как реализовать разбиение по страницам данных эффективный](# "реализации эффективного данных разбиения на страницы")
9. [Защита приложений с использованием проверки подлинности и авторизации](# "безопасных приложений с использованием проверки подлинности и авторизации")
10. [Использование AJAX для динамического обновления](# "использовать AJAX для динамического обновления")
11. [Использование AJAX для реализации сценариев сопоставления](# "использовать AJAX для реализации сценариев сопоставления")
12. [Как включить автоматическое тестирование модулей](# "включить автоматические модульное тестирование")

Можно создать собственную копию обновление NerdDinner с нуля, выполнив каждый шаг мы Пошаговое руководство в этой главе. Кроме того, можно загрузить полную версию исходного кода здесь: [обновление NerdDinner на GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Можно также Дополнительно [загрузить бесплатную версию PDF этого учебника](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Если вы хотите прочитать учебник в автономном режиме.

Visual Studio 2008 или произвольным Visual Web Developer 2008 Express можно использовать для построения приложения. Для базы данных можно использовать SQL Server или произвольным SQL Server Express.

Можно установить ASP.NET MVC, Visual Web Developer 2008 Express и SQL Server Express (бесплатно) с помощью V2 из [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Теперь давайте начнем...

Теперь, когда мы рассмотрели, что такое обновление NerdDinner, давайте нашей рукава и написать код.

Начнем с помощью файла -&gt;новый проект в Visual Studio для создания приложения обновление NerdDinner.

>[!div class="step-by-step"]
[Вперед](create-a-new-aspnet-mvc-project.md)
