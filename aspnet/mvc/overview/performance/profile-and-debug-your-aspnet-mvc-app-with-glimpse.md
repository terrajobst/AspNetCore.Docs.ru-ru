---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Профилирование и отладке приложения ASP.NET MVC с помощью Glimpse | Документы Microsoft
author: Rick-Anderson
description: Обзор — бурно и семейству пакетов NuGet открытым исходным кодом, предоставляющий подробные производительности, отладки и диагностических сведений для ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872979"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Профилирование и отладке приложения ASP.NET MVC с помощью Glimpse
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Обзор — это бурно и семейству пакетов NuGet открытым исходным кодом, предоставляющий подробные производительности, отладки и диагностические сведения для приложений ASP.NET. Тривиальное для установки, простой и высокоскоростные и отображает ключевые показатели производительности в нижней части каждой страницы. Он позволяет углубляться в приложения, если необходимо узнать, что происходит на сервере. Обзор предоставляет намного ценную информацию, мы рекомендуем использовать ее в течение всего цикла разработки, включая Azure тестовой среды. Во время [Fiddler](http://www.telerik.com/fiddler) и [средства разработки F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) предоставляют стороны клиента представления, Glimpse предоставляет подробную информацию с сервера. Этот учебник основное внимание уделяется с помощью Glimpse ASP.NET MVC и EF пакеты, но доступны другие пакеты. По возможности будет связывать к соответствующему [знакомы docs](http://getglimpse.com/Docs/) поддерживать все помогающий. Представление является проекта с открытым кодом, слишком можно взаимодействовать с исходным кодом и документы.


- [Обзор установки](#ig)
- [Включить обзор для localhost](#eg)
- [Вкладка «временная шкала»](#Time)
- [Привязка модели](#mb)
- [Маршруты](#route)
- [С помощью Glimpse в Azure](#da)
- [Дополнительные ресурсы](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Обзор установки

Представление можно установить из консоли диспетчера пакетов NuGet или **управление пакетами NuGet** консоли. Для этой демонстрации я установлю Mvc5 и EF6 пакеты:

![Установка из NuGet Dlg краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Поиск *Glimpse.EF*

![Glimpse.EF из dlg Установка NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Выбрав **установленных пакетов**, вы увидите краткого описания зависимые модули, установленные:

![Установить пакеты Glimpse из DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Следующие команды устанавливают MVC5 краткого описания и EF6 модулей с помощью консоли диспетчера пакетов:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Включить обзор для localhost

Перейдите к http://localhost: &lt;порт #&gt;/glimpse.axd и нажмите кнопку <strong>Включение краткого описания</strong> кнопки.

![Страница axd краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Если отображается панель избранного, можно перетаскивать и drop краткого описания кнопок и добавлять их в виде bookmarklets:

![IE с boookmarklets краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Теперь можно переходить приложения и **головок копирование отображения** (HUD), показаны в нижней части страницы.

![Страница диспетчера контактов с HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[HUD краткого описания страницы](http://getglimpse.com/Docs/Heads-up-Display) подробные сведения о времени, показанном выше. Отображение данных HUD ненавязчивого производительности может уведомить проблемах немедленно - до перехода цикл тестирования. Щелкнув &quot;g&quot; в правом нижнем углу появится в панели «Обзор»:

![Обзор панели](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

На рисунке выше [вкладку выполнение](http://getglimpse.com/Docs/Execution-Tab) выбран, который демонстрирует сведений о времени действия и фильтров в конвейере. Вы увидите my [остановить Контрольное значение фильтра таймера](http://www.nuget.org/packages/StopWatch/) запустить на этапе 6 конвейера. Мой упрощенная таймера можно предоставляют полезные профиля и синхронизации данных, он промахов время, затраченное на авторизации и отображении представления. Вы найдете сведения о моем таймера в [профиля и время приложения ASP.NET MVC вплоть до Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Вкладки](http://getglimpse.com/Docs/Tabs) страница содержит ссылки на подробные сведения на каждой вкладке.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Вкладка «временная шкала»

Я изменил Tom Dykstra необработанных [EF 6/MVC 5 учебника](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) с помощью следующего кода будет сделать контроллера преподавателей:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Приведенный выше код позволяет передать в строке запроса (`eager`) для управления eager или явную загрузку данных. На рисунке ниже, используется явная загрузка и время страницы показывает каждый регистрации, загруженных в `Index` метода действия:

![явная загрузка](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

В следующем коде задается eager и регистрации каждой выборке после `Index` представление называется:

![Указанный Eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Наводите указатель мыши на сегмент времени, чтобы получить подробные сведения о времени:

![При наведении указателя мыши, чтобы просмотреть подробные сведения о времени](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Привязка модели

[Модель вкладку привязки](http://getglimpse.com/Docs/Model-Binding-Tab) предоставляет достаточные сведения помогут вам понять, как привязаны к переменным формы и почему некоторые не привязаны, как и ожидалось. На рисунке ниже показана **?** значок, который можно щелкнуть для открытия страницы справки краткого описания для данного компонента.

![Привязка представление модели краткого описания](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Маршруты

 Вкладка маршруты краткого описания будет может помочь отладки и понимания маршрутизации. На приведенном ниже рисунке выбран маршрут продукции (и зеленым, соглашение краткого описания цветом). ![Выбранное имя продукта](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) также отображаются маркеры ограничения, областей и данных маршрута. В разделе [Glimpse маршруты](http://getglimpse.com/Docs/Routes-Tab) и [атрибут маршрутизации в ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) для получения дополнительной информации. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>С помощью Glimpse в Azure

Обзор политики безопасности позволяет только краткого описания данных для отображения из локального узла. Эта политика безопасности могут изменяться, поэтому эти данные можно просмотреть на удаленном сервере (например, веб-приложение в Azure). Для тестовой среды в Azure, добавьте выделенные метки до нижней части *web.confg* файл, чтобы включить Обзор:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Только после этого изменения любой пользователь может видеть данные краткого описания на удаленном узле. Рассмотрите возможность добавления разметки выше профиль публикации, поэтому она развернута только примененного при использовании этого профиля публикации (например, ваш Azure теста proifle.) Чтобы ограничить представление данных, мы добавим `canViewGlimpseData` роли и только пользователи с этой ролью, для просмотра краткого описания данных.

Удаление комментариев из *GlimpseSecurityPolicy.cs* и измените [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) вызов из `Administrator` для `canViewGlimpseData` роли:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Безопасность – большого объема данных, предоставляемые краткого описания может предоставить доступ к безопасности приложения. Корпорация Майкрософт не выполнил аудита безопасности компонента Glimpse для использования в приложениях производства.


Сведения о добавлении ролей см. в разделе Мои [развернуть веб-приложении Secure ASP.NET MVC 5 с членством, OAuth и базы данных SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) учебника.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Развертывание приложения безопасного ASP.NET MVC 5 с членством, OAuth и базы данных SQL в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Знакомы конфигурации](http://getglimpse.com/Docs/Configuration) -Doc страницы о настройке вкладки, политику среды выполнения, ведение журнала и многое другое.
