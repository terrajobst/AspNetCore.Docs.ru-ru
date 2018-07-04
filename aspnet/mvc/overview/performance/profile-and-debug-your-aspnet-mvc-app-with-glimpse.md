---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Профилирование и отладка приложения ASP.NET MVC с помощью Glimpse | Документация Майкрософт
author: Rick-Anderson
description: Glimpse — бурно и семейству пакетов NuGet с открытым кодом, предоставляющий подробные сведения о производительности, отладку и диагностических сведений для ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: ef65b512645d3f013ea34d3557da93254425c419
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391074"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Профилирование и отладка приложения ASP.NET MVC с помощью Glimpse
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Glimpse — бурно и семейству пакетов NuGet с открытым кодом, предоставляющий подробные сведения о производительности, отладку и диагностические сведения для приложений ASP.NET. Он просто установить, простой и сверхбыстрой и отображает ключевые показатели производительности в нижней части каждой страницы. Он дает возможность углубиться в приложении, если вам нужно узнать, что происходит на сервере. Glimpse предоставляет намного ценную информацию, мы рекомендуем использовать его на протяжении всего цикла разработки, включая Azure тестовой среды. Хотя [Fiddler](http://www.telerik.com/fiddler) и [средства разработки F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) предоставляют на стороне клиента представления, представление предоставляет подробную информацию с сервера. Этот учебник посвящен с помощью Glimpse ASP.NET MVC и EF пакеты, но доступны многие другие пакеты. По возможности я будет связан соответствующий [Окиньте документация](http://getglimpse.com/Docs/) которого я целях. Представление — это проект с открытым исходным кодом, слишком у вас есть доступ к исходному коду и документы.


- [Установка краткого описания](#ig)
- [Включить краткого описания для localhost](#eg)
- [На вкладке временной шкалы](#Time)
- [Привязка модели](#mb)
- [Маршруты](#route)
- [С помощью Glimpse в Azure](#da)
- [Дополнительные ресурсы](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Установка краткого описания

Представление можно установить из консоли диспетчера пакетов NuGet или **управление пакетами NuGet** консоли. Для этой демонстрации я установлю Mvc5 и EF6 пакеты:

![Установка Glimpse из NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Поиск *Glimpse.EF*

![Glimpse.EF из NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Выбрав **установленные пакеты**, вы увидите Glimpse зависимые модули, установленные:

![Установленные пакеты Glimpse из DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Следующие команды устанавливают модули Glimpse MVC5 и EF6 из консоли диспетчера пакетов:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Включить краткого описания для localhost

Перейдите к http://localhost:&lt; порт #&gt;/glimpse.axd и нажмите кнопку <strong>Включение Glimpse</strong> кнопки.

![Glimpse axd страницы](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

При наличии отображается панель избранного, можно перетащить и drop краткого описания кнопок и добавлять их в виде bookmarklets:

![IE с помощью Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Приложения, теперь можно перейти и **головок до отображения** (HUD) отображается в нижней части страницы.

![Страница диспетчера контактов с HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse HUD страницы](http://getglimpse.com/Docs/Heads-up-Display) указана информация о времени, показанный выше. Отображение данных HUD ненавязчивого производительности может уведомить о проблемах немедленно - до перехода к цикл тестирования. Щелкнув &quot;g&quot; в правом нижнем углу появится в панели «представление»:

![Обзор панели](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

На рисунке выше [вкладку выполнение](http://getglimpse.com/Docs/Execution-Tab) выбран, показывающий сведений о времени для действий и фильтры в конвейере. Можно увидеть мой [Watch остановить фильтр таймера](http://www.nuget.org/packages/StopWatch/) запустить на этапе 6 конвейера. Хотя моей таймера небольшого размера могут обеспечить полезные профиля и синхронизации данных, она игнорирует все время, затраченное на авторизации и отображении представления. Сведения о моем таймера в [профиля и времени приложения ASP.NET MVC, вплоть до Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Вкладки](http://getglimpse.com/Docs/Tabs) странице представлены ссылки на подробные сведения на каждой вкладке.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>На вкладке временной шкалы

Я изменил том Дайкстра необработанных [учебник по EF MVC 5,6/](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) следующим кодом Сменить контроллер instructors:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Приведенный выше код позволяет мне передавать в строке запроса (`eager`) к элементу управления, упреждающая и явная загрузка данных. В приведенном ниже рисунке используется явная загрузка и на странице о времени отображаются каждой регистрации, загруженных в `Index` метода действия:

![явная загрузка](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

В следующем коде задается Безотложная, и каждой регистрации извлекается после `Index` представление называется:

![Eager указан](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Наводите указатель мыши на сегмент времени, чтобы получить подробные сведения о времени:

![При наведении указателя мыши, чтобы просмотреть подробные сведения о времени](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Привязка модели

[Вкладке привязки модели](http://getglimpse.com/Docs/Model-Binding-Tab) предоставляет широкий набор сведений, которые помогут вам понять, как связаны переменных формы и почему некоторые не привязаны, как ожидал. На рисунке ниже показано **?** значок, который можно щелкнуть для открытия страницы справки краткого описания для этой функции.

![окиньте представление привязки модели](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Маршруты

 На вкладке Glimpse маршруты можно поможет отладки и понимания маршрутизации. В приведенном ниже рисунке продукта маршрут выбирается (и показывает зеленым цветом, соглашение Glimpse). ![Имя продукта, выбранное](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) также отображаются маркеры ограничения, областей и данных маршрута. См. в разделе [маршруты Glimpse](http://getglimpse.com/Docs/Routes-Tab) и [маршрутизации в ASP.NET MVC 5 с помощью атрибутов](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Дополнительные сведения. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>С помощью Glimpse в Azure

Политика безопасности по умолчанию представление позволяет только представление данных для отображения из локального узла. Можно изменить эту политику безопасности, чтобы можно было просматривать эти данные на удаленном сервере (например, веб-приложения в Azure). Для сред тестирования в Azure, добавьте знак выделенный до нижней части *web.confg* файл, чтобы включить представление:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Только после этого изменения любой пользователь может видеть представление данных на удаленном узле. Рассмотрите возможность добавления разметки выше профиль публикации, поэтому только развертывания примененного, при использовании этого профиля публикации (например, ваш тестирования Azure proifle.) Для ограничения доступа к данным краткого описания, мы добавим `canViewGlimpseData` роли и только пользователи с этой ролью, чтобы просмотреть представление данных.

Удалить комментарии из *GlimpseSecurityPolicy.cs* файлов и изменить [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) вызов из `Administrator` для `canViewGlimpseData` роли:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Безопасность — мультимедийными данными, предоставляемые Glimpse может привести к безопасности приложения. Майкрософт не производит аудита безопасности из краткого описания для использования в приложениях для производства.


Сведения о добавлении ролей, см. в разделе my [развертывание веб-приложения Secure ASP.NET MVC 5 с членством, OAuth и базой данных SQL в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) руководства.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Развертывание безопасного приложения ASP.NET MVC 5 с членством, OAuth и базой данных SQL в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Окиньте конфигурации](http://getglimpse.com/Docs/Configuration) -Doc страницы о настройке вкладок, политику среды выполнения, ведение журнала и многое другое.
