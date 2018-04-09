---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Добавление ASP.NET Identity пустой или существующий веб-форм проекта | Документы Microsoft
author: raquelsa
description: Этот учебник демонстрирует добавление ASP.NET Identity (новый система членства для ASP.NET) для приложения ASP.NET. При создании нового веб-форм или MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8961e596f0d6cc4810e2439be1ec2915bddb8c78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Добавление ASP.NET Identity пустой или существующий веб-форм проекта
====================
по [Raquel Soares De Almeida](https://github.com/raquelsa)

> Этот учебник демонстрирует добавление [ASP.NET Identity](introduction-to-aspnet-identity.md) (новый система членства для ASP.NET) для приложения ASP.NET.
> 
> При создании нового проекта веб-форм или MVC в Visual Studio 2013 RTM с отдельным учетным записям Visual Studio установите все необходимые пакеты и добавить все необходимые классы для вас. Этот учебник демонстрирует шаги для добавления поддержки ASP.NET Identity существующий проект веб-форм или новый пустой проект. Я будут указаны все пакеты NuGet, которые необходимо установить и классы, которые необходимо добавить. Я будут рассмотрены образец веб-форм для входа в систему при выделении все основные элементы точки API-интерфейсы для управления пользователями и проверки подлинности и регистрации новых пользователей. Этот образец использует реализацию по умолчанию ASP.NET Identity для хранения данных SQL, построенной на основе Entity Framework. Этого учебника мы будем использовать LocalDB для базы данных SQL.
> 
> Этот учебник был записан с Raquel Soares De Almeida и Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Приступая к работе ASP.NET Identity

1. Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Нажмите кнопку **новый проект** от начала страницы, или можно использовать меню и выберите **файл**, а затем **новый проект**.
3. Выберите **Visual C# i** n левой панели, затем **Web** , а затем выберите **веб-приложение ASP.NET**. Назовите проект «WebFormsIdentity» и нажмите кнопку **ОК**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. В **новый проект ASP.NET** диалогового окна выберите **пустой** шаблона.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Обратите внимание **изменить аутентификацию** кнопка становится недоступной, и без проверки подлинности не поддерживается в этом шаблоне. Шаблоны веб-форм, MVC и веб-API позволяют выбрать способ проверки подлинности. Дополнительные сведения см. в разделе [обзор проверки подлинности](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Добавление идентификаторов пакетов для приложения

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**. В диалоговом окне поле текст поиска, введите «*Identity.E*». Нажмите "установить" для этого пакета.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Обратите внимание, что этот пакет будут установлены пакеты зависимостей: EntityFramework и Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Добавление веб-формы для регистрации пользователей

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и нажмите кнопку **добавить**, а затем **веб-формы**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. В **указания имени элемента** диалоговом имя новой веб-формы **зарегистрировать**, а затем нажмите кнопку **ОК**
3. Замените существующую разметку в созданном *Register.aspx* файла следующим кодом. Изменения в коде выделены.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Это просто упрощенную версию *Register.aspx* файл, который создается при создании нового проекта веб-форм ASP.NET. Показанная выше разметка добавляет поля формы и кнопку, чтобы зарегистрировать нового пользователя.
4. Откройте *Register.aspx.cs* и замените содержимое файла следующим кодом:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Приведенный выше код является упрощенной версией *Register.aspx.cs* файл, который создается при создании нового проекта веб-форм ASP.NET.
    > 2. *IdentityUser* класс является реализацией по умолчанию EntityFramework *IUser* интерфейса. *IUser* интерфейс — минимальный интерфейс пользователя в ASP.NET Identity Core.
    > 3. *UserStore* класс является реализацией EntityFramework по умолчанию хранилища пользователя. Этот класс реализует ASP.NET Identity Core минимальным количеством интерфейсов: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* и *IUserRoleStore* .
    > 4. *UserManager* класс предоставляет связанный с пользователем API-интерфейсов, который автоматически сохранит изменения *UserStore*.
    > 5. *IdentityResult* класса представляет результат операции identity.
5. В **обозревателе решений**, щелкните правой кнопкой мыши проект и нажмите кнопку **добавить**, **добавить папку ASP.NET** и затем **приложения\_данные**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Откройте *Web.config* и добавьте строку подключения для базы данных, мы будем использовать для хранения информации о пользователях. Базы данных создается во время выполнения, EntityFramework для идентификации сущности. Строка подключения похожее создается при создании нового проекта веб-форм. Выделенный код показана разметка, что следует добавить:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Для Visual Studio 2015 или более поздней версии, замените `(localdb)\v11.0` с `(localdb)\MSSQLLocalDB` в строке подключения.
    
7. Щелкните правой кнопкой мыши файл *Register.aspx* в проект и выберите **задать в качестве начальной страницы**. Нажмите клавиши Ctrl + F5 для построения и запуска веб-приложения. Введите новое имя пользователя и пароль и нажмите кнопку на **зарегистрировать**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity поддерживает проверки, и в этом образце можно проверить поведение по умолчанию имя пользователя и пароль проверяющие элементы управления, поступающих из идентификаторов основного пакета. Проверяющий элемент управления по умолчанию для пользователя (`UserValidator`) имеет свойство `AllowOnlyAlphanumericUserNames` , имеет значение по умолчанию равно `true`. Проверяющий элемент управления по умолчанию для пароля (`MinimumLengthValidator`) обеспечивает пароля, по крайней мере 6 символов. Эти проверки включены свойства `UserManager` , может быть переопределен, если вы хотите использовать пользовательской проверки

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Проверка базы данных LocalDb удостоверений и таблицы, сформированные платформой Entity Framework

1. В **представление** меню, нажмите кнопку **обозревателя серверов**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Разверните **DefaultConnection (WebFormsIdentity)**, разверните **таблиц**, щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Настройка приложения для проверки подлинности OWIN

На этом этапе только добавлена поддержка для создания пользователей. Теперь мы собираемся продемонстрировать, каким образом можно добавить проверки подлинности для входа в систему пользователя. ASP.NET Identity использует проверку подлинности Microsoft OWIN по промежуточного слоя для проверки подлинности форм. Проверка подлинности файла Cookie OWIN куки-файл и требует проверки подлинности на основе механизм, который может использоваться любой платформой, размещенных на [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) или IIS. С этой моделью и те же пакеты проверки подлинности может использоваться в нескольких платформ, включая ASP.NET MVC и веб-форм. Дополнительные сведения о проекте Katana и выполнения по промежуточного слоя см. в разделе, зависит от узла [Приступая к работе с проектом Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Установка пакетов проверки подлинности для приложения

1. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**. В диалоговом окне поле текст поиска, введите «*Identity.Owin*». Нажмите "установить" для этого пакета.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Выполните поиск пакета ***Microsoft.Owin.Host.SystemWeb*** и установить его.   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** пакет содержит набор классов расширения OWIN по промежуточного слоя OWIN проверки подлинности для использования пакетов ASP.NET Identity Core настраивать и управлять ими.  
    > **Microsoft.Owin.Host.SystemWeb** пакет содержит сервере OWIN, который позволяет приложениям на основе OWIN для запуска в конвейере ASP.NET с помощью IIS. Дополнительные сведения см. [по промежуточного слоя OWIN в службах IIS интегрирован конвейера](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Добавление запуска OWIN и классы конфигурации проверки подлинности

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект, нажмите кнопку **добавить**, а затем **Добавление нового элемента**. В диалоговом окне поле текст поиска, введите «*owin*». Имя класса "*запуска*» и нажмите кнопку **добавить**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. В файле Startup.cs-файле добавьте выделенный код, показано ниже, чтобы настроить проверку подлинности файла cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Этот класс содержит `OwinStartup` атрибут для указания класса запуска OWIN. Каждое приложение OWIN имеет класс запуска, где указываются компоненты для конвейера приложения. В разделе [определение класса запуска OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) Дополнительные сведения об этой модели.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Добавление веб-формы для регистрации и входа пользователей

1. Откройте *Register.cs* и добавьте следующий код, который будет вход пользователя при успешной регистрации файла. Изменения выделены ниже.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Поскольку ASP.NET Identity и проверки подлинности файла Cookie OWIN системы на основе утверждений, платформа требует разработчика приложений для создания [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) для пользователя. ClaimsIdentity приводятся сведения о всех утверждения для пользователя, такие как пользователь принадлежит к роли. На этом этапе также можно добавить дополнительные утверждения для пользователя.
    > - Можно вход пользователя с помощью AuthenticationManager из OWIN и вызывая метод `SignIn` и передавая ClaimsIdentity, как показано выше. Этот код будет вход пользователя и создавать также файл cookie. Этот вызов является аналогом [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) используемые [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) модуля.
2. В **обозревателе решений**, щелкните правой кнопкой мыши щелкните ваш проект **добавить**, а затем **веб-формы**. Имя веб-формы **входа**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Замените содержимое *Login.aspx* файла следующим кодом:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Замените содержимое *Login.aspx.cs* файл со следующим:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Теперь проверяет состояние текущего пользователя и выполняет действие на основе его `Context.User.Identity.IsAuthenticated` состояния.  
    >     **Отображения регистрации в имени пользователя** : Microsoft ASP.NET Identity Framework перечислил методы расширения в [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) , позволяющий получить `UserName` и `UserId` для Пользователь, вошедший в систему. Эти методы расширения определяются в `Microsoft.AspNet.Identity.Core` сборки. Эти методы расширения являются заменой [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Метод входа:   
    >     `This` метод заменяет предыдущий `CreateUser_Click` метод в этот пример и теперь выполняет вход пользователя после успешного создания пользователя.   
    >  Платформа Microsoft OWIN добавил методы расширения `System.Web.HttpContext` , позволяющий получить ссылку на `IOwinContext`. Эти методы расширения определяются в `Microsoft.Owin.Host.SystemWeb` сборки. `OwinContext` Предоставляемых классами `IAuthenticationManager` свойство, которое представляет функциональные возможности по промежуточного слоя проверки подлинности, доступные в текущем запросе.  
    >  Пользователь может войти с помощью `AuthenticationManager` из OWIN и вызывая метод `SignIn` и передача в `ClaimsIdentity` как показано выше.   
    >  Поскольку ASP.NET Identity и проверки подлинности файла Cookie OWIN системы на основе утверждений, платформа требует приложение, чтобы создать `ClaimsIdentity` для пользователя.   
    >  `ClaimsIdentity` Содержит сведения о всех утверждений для пользователя, такие как пользователь принадлежит к роли. На этом этапе также можно добавить дополнительные утверждения для пользователя  
    >  Этот код будет вход пользователя и создавать также файл cookie. Этот вызов является аналогом [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) используемые [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) модуля.
    > - `SignOut` метод:   
    >  Возвращает ссылку на `AuthenticationManager` из OWIN и вызывает `SignOut`. Это аналогично [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) метод, используемый для [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) модуля.
5. Нажмите клавишу **сочетание клавиш Ctrl + F5** для построения и запуска веб-приложения. Введите новое имя пользователя и пароль и нажмите кнопку на **зарегистрировать**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Примечание: на этом этапе новый пользователь создается и вход.
6. Щелкните **Выход** кнопки. Вы будете перенаправлены в журнале в форме.
7. Введите на недопустимое имя пользователя или пароль и нажмите кнопку **входа** кнопки.   
   `UserManager.Find` Метод вернет значение null, а сообщение об ошибке: « *недопустимое имя пользователя или пароль* » будет отображаться.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
