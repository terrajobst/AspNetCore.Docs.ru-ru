---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Документы Microsoft
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 — это платформа для создания масштабируемых, основанную на стандартах веб-приложений с помощью надежных конструктивных шаблонов и эффективных возможностей AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 1b3f920b51a70757ec0e20e36fa8e7dc329e663d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Новые возможности в ASP.NET MVC 5

### <a name="one-aspnet"></a>Один ASP.NET

Шаблоны проектов Web MVC легко интегрировать в новый интерфейс One ASP.NET. Можно настроить проект MVC и настроить проверку подлинности с помощью мастера создания проектов One ASP.NET. Вводное руководство в ASP.NET MVC 5 можно найти в [Приступая к работе с ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Сведения об обновлении проектов MVC 4 до MVC 5 см. в разделе [обновление ASP.NET MVC 4 и API веб-проекта ASP.NET MVC 5 и веб-API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Шаблоны проектов MVC были обновлены для использования ASP.NET Identity для проверки подлинности и управления удостоверениями. Учебник, проверки подлинности Facebook и Google и нового членства API можно найти в [создать приложение ASP.NET MVC 5 с Facebook и Google OAuth2 и входа OpenID](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) и [развертывание приложения ASP.NET MVC защиты с Членство, OAuth и базы данных SQL Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>начальной загрузки

Шаблон проекта MVC был обновлен для использования [начальной загрузки](http://getbootstrap.com/) для предоставления компактный и гибкий внешний вид и поведение, можно легко настроить. Дополнительные сведения см. в разделе [начальной загрузки в веб-шаблоны проектов Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Фильтры проверки подлинности

[Фильтры проверки подлинности](http://www.dotnetcurry.com/showarticle.aspx?ID=957) — это новый тип в ASP.NET MVC, предшествует фильтры авторизации в конвейере ASP.NET MVC и позволяют задавать проверки подлинности логику за действие, фильтр конкретного контроллера, или глобально для всех контроллеров. Фильтры проверки подлинности обрабатывают учетные данные в запросе и укажите соответствующий участник. Фильтры проверки подлинности можно также добавить запросов проверки подлинности в ответ на неавторизованных запросов. В разделе [фильтры проверки подлинности ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [фильтры проверки подлинности в ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Переопределяет фильтр

Теперь можно переопределить, какие фильтры применяются к методу указанного действия или контроллер, указав [переопределение фильтра](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Переопределение фильтры задать фильтр типов, которые не должны выполняться для данной области (действий или контроллера). Это позволяет настроить фильтры, которые применяются глобально, но затем исключить определенные глобальные фильтры от применения определенных действий или контроллеров. В разделе [новый фильтр переопределяет функцию в ASP.NET MVC 5 и ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [том, как использовать ASP.NET MVC 5 переопределяет функцию фильтра,](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), и [переопределяет фильтр в ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Маршрутизация с помощью атрибутов

ASP.NET MVC теперь поддерживает [маршрутизацией атрибутов](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), благодаря вклад по McCall Тим, автор [ http://attributerouting.net ](http://attributerouting.net). С маршрутизацией атрибутов можно указать свои маршруты, сопроводив действия и контроллеры.

## <a name="new-web-project-experience"></a>Новые возможности веб-проекта

Мы обладают улучшенной опыт в создании нового веб-проектов в Visual Studio 2013. В **нового веб-проекта ASP.NET** диалоговом окне можно выбрать тип проекта, настройте любое сочетание технологий (веб-формы, MVC, веб-API), настройки параметров проверки подлинности и добавьте проект модульного теста.

![Новый проект ASP.NET](mvc5/_static/image1.png)

Новое диалоговое окно позволяет изменить параметры проверки подлинности по умолчанию для многих шаблонов. Например при создании проекта веб-форм ASP.NET можно выбрать любой из следующих параметров:

- Без проверки подлинности
- Индивидуальные учетные записи (членства ASP.NET или социальных поставщика вход)
- Учетные записи в организации (Active Directory в веб-приложение)
- Проверка подлинности Windows (Active Directory в приложении интрасети)

![Параметры проверки подлинности](mvc5/_static/image2.png)

Дополнительные сведения о новый процесс для создания веб-проектов см. в разделе [Создание веб-проектов ASP.NET в Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Дополнительные сведения о новых параметров проверки подлинности см. в разделе [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Формирование шаблонов ASP.NET

Формирование шаблонов ASP.NET — это платформа создания кода для веб-приложений ASP.NET. Он позволяет легко добавить стандартный код в проект, который взаимодействует с моделью данных.

В предыдущих версиях Visual Studio формирование шаблонов был ограничен проектов ASP.NET MVC. В Visual Studio 2013 теперь можно использовать формирование шаблонов для любого проекта ASP.NET, включая веб-форм. Visual Studio 2013 не поддерживает создание страниц для проекта веб-форм, но по-прежнему можно использовать формирование шаблонов с веб-формами путем добавления зависимостей MVC в проект. Поддержка создания страниц для веб-формы будет добавлена в будущем обновлении.

При использовании формирования шаблонов мы убедитесь, что все требуемые зависимости устанавливаются в проекте. Например если для создания проекта веб-форм ASP.NET и затем использовать формирование шаблонов для добавления контроллера Web API, необходимые пакеты NuGet и ссылки добавляются в проект автоматически.

Чтобы добавить формирование шаблонов MVC в проект веб-форм, добавьте **новый элемент формирования шаблонов** и выберите **зависимостей MVC 5** в диалоговом окне. Существует два варианта для формирования шаблонов MVC; Минимальными и полное. При выборе минимальной только пакеты NuGet и ссылки для ASP.NET MVC добавляются в проект. При выборе параметра «полная» добавляются минимальные зависимости, а также необходимые файлы содержимого проекта MVC.

Поддержка асинхронных контроллеров формирование шаблонов использует новые функции асинхронного с Entity Framework 6.

Дополнительные сведения и учебники см. в разделе [Обзор формирование шаблонов ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Получение справки и сообщить о возникших проблемах

- [Известные проблемы и критические изменения списка](../visual-studio/overview/2013/release-notes.md#knownissues)
- Получить справку и обсудить ASP.NET MVC 5 в [форумы](https://forums.asp.net/1146.aspx)
- [Отчет об ошибках в ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Выполнение запроса на функцию](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Обновление с ASP.NET MVC 4

В разделе [процедура обновления ASP.NET MVC 4 и веб-проекта ASP.NET MVC 5 и веб-API 2 API](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
