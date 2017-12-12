---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "Введение в ASP.NET Identity | Документы Microsoft"
author: jongalloway
description: "Система членства ASP.NET впервые появилась в ASP.NET 2.0 обратно в 2005 г. и, поскольку затем были много изменений в typicall приложения web способов..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: a66e2a80668dbf291b9cc34f205b546b72d92bcc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-identity"></a>Введение в ASP.NET Identity
====================
по [Джон Гэллоуэй](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> Система членства ASP.NET впервые появилась в ASP.NET 2.0 обратно в 2005 г. и, поскольку то одним из способов проверки подлинности и авторизации веб-приложения обычно обрабатывают произошли изменения многих. ASP.NET Identity рассматривается новую систему членства выполняемые при построении современных приложений для web, телефон или планшет.
> 
> В этой статье было написано с Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Джон Гэллоуэй ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra и Рик Андерсон ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Фон: Членства в ASP.NET

### <a name="aspnet-membership"></a>Членства в ASP.NET

[Членства в ASP.NET](https://msdn.microsoft.com/en-us/library/yh26yfzy(v=VS.100).aspx) предназначена для решения требования членства сайта, которые были распространены в 2005, в которой участвует форм проверки подлинности и базу данных SQL Server для имен пользователей, пароли и данные профиля. Сегодня — гораздо более широкие массив параметров хранилища данных для веб-приложений, и большинство разработчиков нужно разрешить их сайтов использовать Поставщики удостоверений из социальных сетей для функций, аутентификации и авторизации. Ограничения схемы членства ASP.NET затруднить этот переход:

- Схема базы данных было разработано для SQL Server и его невозможно изменить. Можно добавить данные профиля, но дополнительные данные, упакована в другую таблицу, что затрудняет для доступа к любым способом, кроме через API-Интерфейс поставщика профиля.
- В системе поставщика можно изменить хранилище данных, но система созданная на основе предположения для реляционной базы данных. Можно написать поставщика для хранения сведений о членстве в механизм хранения нереляционных данных, таких как таблицы хранилища Azure, но вам придется решить реляционного конструктора путем написания большого объема кода и большой объем `System.NotImplementedException` исключения для методов, которые не применяются к базам данных NoSQL.
- Поскольку функция журнала или журнала вышел основана на аутентификации с помощью форм, не может использовать систему членства [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN включает в себя компоненты по промежуточного слоя для проверки подлинности, включая поддержку для журнала модулей с помощью внешних поставщиков (например, учетные записи Microsoft, Facebook, Google, Twitter) и журнала модули с учетными записями организации из локальной Active Directory или Azure Active Directory. OWIN также поддерживает OAuth 2.0, JWT и CORS.

### <a name="aspnet-simple-membership"></a>Членство ASP.NET Simple

[Простого членства ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) разработано как система членства в ASP.NET Web Pages. Он был выпущен с WebMatrix и Visual Studio 2010 с пакетом обновления 1. Цель простого членства заключалась в том, чтобы облегчить Добавление функции членства в веб-страницы приложения.

Простого членства упрощают для настройки сведения о профиле пользователя, но она по-прежнему имеет другие проблемы с помощью членства ASP.NET, и он имеет некоторые ограничения:

- Было трудно сохранять данные системы членства в-реляционного хранилища.
- Его нельзя использовать с OWIN.
- Оно плохо работает с существующие поставщики членства ASP.NET, а не расширяется.

### <a name="aspnet-universal-providers"></a>Универсальные поставщики ASP.NET

[Универсальные поставщики ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) были разработаны, чтобы обеспечить возможность сохранения данных членства в Microsoft, база данных SQL Azure и они также работают с SQL Server Compact. Универсальные поставщики были построены в Entity Framework Code First, это означает, что универсальные поставщики может использоваться для сохранения данных в любом магазине, поддерживаемых EF. С универсальные поставщики схемы базы данных была очищена много также.

Универсальные поставщики встроены в инфраструктуре членства ASP.NET, и они по-прежнему содержат те же ограничения, как поставщик SqlMembership. То есть они предназначались для реляционных баз данных и будет сложно настроить профиль и сведения о пользователях. Эти поставщики по-прежнему использовать проверку подлинности форм журнала и журнала.

## <a name="aspnet-identity"></a>ASP.NET Identity

Как членство истории в ASP.NET имел годы, команда ASP.NET научились из отзывов от клиентов.

Предполагается, что пользователи будут входить в систему, указав имя пользователя и пароль, которые они зарегистрированы в приложении больше не является допустимым. Веб-сайте стала более социальных сетей. Пользователи взаимодействуют друг с другом в режиме реального времени через социальных каналов, например Facebook, Twitter и других социальных веб-сайтов. Разработчики предоставляют пользователям зарегистрироваться с использованием удостоверения социальных сетей, чтобы они могут иметь более удобной на своих веб-сайтах. Система современных членства необходимо включить перенаправление журнала систему на основе для поставщиков проверки подлинности Facebook, Twitter и другие.

Как развитие веб-разработки, поэтому не шаблонов разработки веб-приложений. Модульных тестов для кода приложения стала ключевое соображение для разработчиков приложений. 2008 в ASP.NET добавлена новая платформа на основе шаблона Model-View-Controller (MVC), частично, чтобы помочь разработчикам создавать модульные тестируемых приложений ASP.NET. Разработчики, желающие модульного тестирования логики своих приложений, также необходимо иметь возможность сделать с помощью системы членства.

Учитывая эти изменения в разработке веб-приложений ASP.NET Identity был разработан для следующих целей:

- **Одной системы ASP.NET Identity**

    - ASP.NET Identity можно использовать со всеми платформы ASP.NET, такие как ASP.NET MVC, веб-форм, веб-страницы, веб-API и SignalR.
    - При построении телефона, хранилища, гибридное или веб-приложениях можно использовать ASP.NET Identity.
- **Простота подключения профиля данные о пользователе**

    - Вы можете контролировать схемы пользователя и сведения о профиле. Например можно легко включить системой для хранения даты рождения, вводимые пользователем при регистрации учетной записи в приложении.

- **Сохранение состояния элемента управления**

    - По умолчанию система ASP.NET Identity сохраняет все сведения о пользователе в базе данных. ASP.NET Identity используется Entity Framework Code First для реализации всех механизма сохраняемости.
    - Так как вы управляете схемы базы данных, общие задачи, такие как изменение имен таблиц или тип данных первичные ключи несложно.
    - Легко подключать в другое хранилище механизмы, такие как SharePoint, таблицы хранилища Azure, базах данных NoSQL, т. д., без необходимости создавать `System.NotImplementedExceptions` исключения.
- **Возможности модульного тестирования**

    - ASP.NET Identity делает веб-приложения больше единицы тестируемых. Можно создавать модульные тесты для части приложения, использующие ASP.NET Identity.
- **Поставщик ролей**

    - Нет поставщика ролей, который позволяет ограничить доступ к части приложения с помощью ролей. Можно легко создавать роли, например «Admin» и добавьте пользователей в роли.
- **На основе утверждений**

    - ASP.NET Identity поддерживает аутентификацию, на основе утверждений, где удостоверение пользователя представляется как набор утверждений. Утверждения позволяют разработчикам быть намного более выразительным при описании идентификатора пользователя, чем разрешено ролей. Членство в роли имеет только логическое значение (члена или члена, не являющегося), утверждение может включать подробные сведения об удостоверении пользователя и членства.
- **Поставщиков социальных сетей входа**

    - Можно легко добавить в приложение социальных модули журнала учетной записи Microsoft, Facebook, Twitter, Google и другие и хранения пользовательских данных в приложении.
- **Azure Active Directory**

    - Также можно добавить функциональность в журнал с помощью Azure Active Directory и хранения пользовательских данных в приложении. Дополнительные сведения см. в разделе [учетные записи организации](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) в создание веб-проектов ASP.NET в Visual Studio 2013
- **Интеграция OWIN**

    - Проверка подлинности ASP.NET теперь основан на по промежуточного слоя OWIN, который можно использовать на любом узле на основе OWIN. ASP.NET Identity нет какой-либо зависимостью System.Web. Он представляет собой полностью соответствует OWIN и может использоваться в любом приложении размещенных OWIN.
    - ASP.NET Identity OWIN проверки подлинности использует для журнала или журнала вышел пользователей на веб-сайте. Это означает, что вместо использования FormsAuthentication для создания файла cookie, приложение использует OWIN CookieAuthentication для этого.
- **Пакет NuGet**

    - ASP.NET Identity перераспределяется как пакет NuGet, который устанавливается в шаблонах ASP.NET MVC, веб-форм и веб-API, которые поставляются вместе с Visual Studio 2013. Этот пакет NuGet можно загрузить из галереи NuGet.
    - Освобождение ASP.NET Identity как NuGet пакета упрощает для ASP.NET командой итерацию в новые функции и исправления ошибок и поставлять эти разработчикам самостоятельно.

## <a name="getting-started-with-aspnet-identity"></a>Приступая к работе с ASP.NET Identity

ASP.NET Identity используется в шаблонах проектов Visual Studio 2013 для ASP.NET MVC, веб-форм, веб-API и SPA. В этом пошаговом руководстве мы будет иллюстрируют использование ASP.NET Identity шаблоны проектов для расширения функциональности для регистрации, вход и выход пользователя.

ASP.NET Identity реализуется с помощью следующей процедуры. Цель этой статьи — предоставить общий обзор ASP.NET Identity; можно, выполните его шаг за шагом или просто просмотра подробных сведений. Более подробные инструкции по созданию приложений с помощью ASP.NET Identity, в том числе с помощью нового API для добавления пользователей роли и сведения профиля, в разделе далее действия в конце этой статьи.

1. Создание приложения ASP.NET MVC с отдельным учетным записям. Можно использовать ASP.NET Identity в ASP.NET MVC, веб-форм, веб-API, SignalR и т. д. В этой статье мы начнем с приложением ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Созданный проект содержит следующие три пакета для ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 Этот пакет содержит реализацию платформы Entity Framework удостоверения ASP.NET, который будет сохраняться данные ASP.NET Identity и схему для SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 Этот пакет имеет базовых интерфейсов для ASP.NET Identity. Этот пакет можно использовать для записи реализацию для ASP.NET Identity, сохраняемости на разных целевых объектов хранит Azure таблицы, например хранилище NoSQL баз данных и т. д.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 Этот пакет содержит функции, используемое для подключения в OWIN проверки подлинности с использованием удостоверения ASP.NET в приложениях ASP.NET. Это используется при добавлении журнала функциональных возможностей приложения и вызов для создания файла cookie проверки подлинности файла Cookie OWIN по промежуточного слоя.
3. Создание пользователя.  
 Запустите приложение и щелкните на **зарегистрировать** ссылку, чтобы создать пользователя. Ниже приведен на страницу регистрации, которая собирает имя пользователя и пароль.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 Когда пользователь щелкает **зарегистрировать** кнопки `Register` контроллера учетных записей создается пользователь, вызвав API ASP.NET удостоверения, как показано ниже:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Войти.  
 Если пользователь успешно создан, она регистрирует `SignInAsync` метод.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 Выделенный код выше в `SignInAsync` метод создает [ClaimsIdentity](https://msdn.microsoft.com/en-us/library/system.security.claims.claimsidentity.aspx). Поскольку ASP.NET Identity и проверки подлинности файла Cookie OWIN системы на основе утверждений, платформа требует приложение для создания ClaimsIdentity для пользователя. ClaimsIdentity приводятся сведения о всех утверждений для пользователя, такие как пользователь принадлежит к роли. На этом этапе также можно добавить дополнительные утверждения для пользователя.  
  
 Выделенный код ниже в `SignInAsync` метод входит в пользователя с помощью AuthenticationManager из OWIN и вызывая метод `SignIn` и передавая ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Выйдите из системы.  
 Щелкнув **выйти** ссылку вызывает действие выхода в контроллера учетных записей. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 Выделенный код выше OWIN `AuthenticationManager.SignOut` метод. Это аналогично [FormsAuthentication.SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx) метод, используемый для [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) модуля в веб-форм.

## <a name="components-of-aspnet-identity"></a>Компоненты ASP.NET Identity

На приведенной ниже схеме показаны компоненты системы ASP.NET Identity (щелкните [это](introduction-to-aspnet-identity/_static/image3.png) или на диаграмме, чтобы увеличить его). Пакеты зеленым цветом составляют системы ASP.NET Identity. Другие пакеты, зависимости, необходимые для использования системы удостоверений ASP.NET в приложениях ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Вот краткое описание пакетов NuGet, не упомянутые ранее:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 По промежуточного слоя, который позволяет приложению использовать куки-файл на основе проверки подлинности, аналогично ASP. NET проверки подлинности форм.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Платформа Entity Framework — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Миграция из членства в ASP.NET Identity

Мы надеемся скоро представляют собой руководства по переносу существующих приложений, использующих членства в ASP.NET или простого членства в новую систему ASP.NET Identity.

## <a name="next-steps"></a>Дальнейшие действия

- [Создание приложения ASP.NET MVC 5 с Facebook и Google OAuth2 и входа OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Учебник использует API удостоверения ASP.NET для добавления сведения профиля к пользовательской базе данных, а также для проверки подлинности в Google и Facebook.
- [Создать приложение ASP.NET MVC с помощью проверки подлинности и баз данных SQL Server и развернуть в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Этого учебника показано, как использовать API удостоверения для добавления пользователей и ролей.
- [Учетные записи отдельных пользователей](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) при создании проектов веб-ASP.NET в Visual Studio 2013
- [Учетные записи организации](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) при создании проектов веб-ASP.NET в Visual Studio 2013
- [Настройка данных профиля в ASP.NET Identity в шаблонах Visual STUDIO 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Дополнительные сведения можно получить от поставщиков социальных сетей, используемых в шаблоны проектов Visual STUDIO 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Образец приложения, которое показывает, как добавить основные роли и поддержка пользователей и как это делать, ролей и управление пользователями.
