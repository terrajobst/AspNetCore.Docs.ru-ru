---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Добавление ASP.NET Identity к пустой или существующий веб-проект форм | Документация Майкрософт
author: raquelsa
description: Этом руководстве показано, как добавление ASP.NET Identity (новая система членства для ASP.NET) в приложение ASP.NET. При создании нового веб-форм или MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236514"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Добавление ASP.NET Identity в пустой или существующий веб-проект форм


> Этот учебник демонстрирует добавление [ASP.NET Identity](introduction-to-aspnet-identity.md) (новая система членства для ASP.NET) для приложений ASP.NET.
> 
> При создании нового проекта веб-форм или MVC в Visual Studio 2017 RTM отдельные учетные записи Visual Studio установите все необходимые пакеты и добавить все необходимые классы для вас. Этот учебник будет покажем, как добавить поддержку ASP.NET Identity в существующий проект веб-форм или новый пустой проект. Я рассмотрю все пакеты NuGet, которые необходимо установить и классы, которые необходимо добавить. Я будут рассмотрены пример веб-форм для регистрации новых пользователей и вход в систему, выделите все основные элементы точки API-интерфейсы для управления пользователями и проверки подлинности. В этом примере будет использоваться реализация по умолчанию ASP.NET Identity для хранилища данных SQL, которая построена на платформе Entity Framework. Этом руководстве мы будем использовать LocalDB для базы данных SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Начало работы с ASP.NET Identity

1. Начните с установки и запуска [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Выберите **новый проект** от начала страницы, или можно использовать меню и выберите **файл**, а затем **новый проект**.
3. В левой области разверните **Visual C#** , а затем выберите **Web**, затем **веб-приложение ASP.NET (.Net Framework)**. Присвойте проекту имя «WebFormsIdentity» и выберите **ОК**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. В **новый проект ASP.NET** диалоговом окне выберите **пустой** шаблона.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Обратите внимание, что **изменить способ проверки подлинности** кнопка отключена, поддержка проверки подлинности, не предоставляется в этом шаблоне. Шаблоны веб-форм, MVC и веб-API позволяют выбрать подход к проверке подлинности.

## <a name="add-identity-packages-to-your-app"></a>Добавление пакетов удостоверений в приложение

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**. Найдите и установите **Microsoft.AspNet.Identity.EntityFramework** пакета. 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Обратите внимание на то, что этот пакет устанавливается пакеты зависимостей: **EntityFramework** и **Core Microsoft ASP.NET Identity**.

## <a name="add-a-web-form-to-register-users"></a>Добавьте веб-форму для регистрации пользователей

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить**, а затем **веб-формы**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. В **указания имени элемента** диалоговом окне имя новой веб-формы **зарегистрировать**, а затем выберите **ОК**
3. Замените существующую разметку в созданном *Register.aspx* файла следующим кодом. Изменения в коде выделены. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Это просто упрощенную версию *Register.aspx* файл, который создается при создании нового проекта веб-форм ASP.NET. Приведенная выше разметка добавляет поля формы и кнопку для регистрации нового пользователя.
4. Откройте *Register.aspx.cs* файл и замените содержимое файла следующим кодом:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Приведенный выше код представляет упрощенную версию *Register.aspx.cs* файл, который создается при создании нового проекта веб-форм ASP.NET.
    > 2. *IdentityUser* класс является реализацией по умолчанию EntityFramework *IUser* интерфейс. *IUser* это минимальный интерфейс для пользователя на удостоверение ASP.NET Core.
    > 3. *UserStore* класс является реализацией EntityFramework по умолчанию из хранилища пользователя. Этот класс реализует минимальным количеством интерфейсов удостоверения ASP.NET Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* и *IUserRoleStore*.
    > 4. *UserManager* класс предоставляет связанный с пользователем API-интерфейсов, который автоматически сохранит изменения *UserStore*.
    > 5. *IdentityResult* класса представляет результат операции identity.
5. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить**, **добавить папку ASP.NET** и затем **приложения\_данных**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Откройте *Web.config* файл и добавьте запись строки соединения для базы данных, мы будем использовать для хранения пользовательских данных. Базы данных создается во время выполнения, EntityFramework для идентификации сущностей. Строка подключения похожа на один создаются автоматически при создании проекта веб-форм. Выделенный код показана разметка, который необходимо добавить:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Для Visual Studio 2015 или более поздней версии, замените `(localdb)\v11.0` с `(localdb)\MSSQLLocalDB` в строке подключения.
    
7. Щелкните правой кнопкой мыши файл *Register.aspx* в проект, выберите **задать в качестве начальной страницы**. Нажмите клавиши Ctrl + F5 для сборки и запуска веб-приложения. Введите новое имя пользователя и пароль, а затем выберите **зарегистрировать**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > Удостоверение ASP.NET имеет поддержку проверки и в этом примере можно проверить поведение по умолчанию для пользователя и пароль проверяющие элементы управления, полученные из удостоверения основного пакета. Проверяющий элемент управления по умолчанию для пользователя (`UserValidator`) имеет свойство `AllowOnlyAlphanumericUserNames` , имеет значение по умолчанию, равным `true`. Проверяющий элемент управления по умолчанию для пароля (`MinimumLengthValidator`) гарантирует, что пароль имеет по крайней мере 6 символов. Эти проверяющие элементы управления являются свойствами на `UserManager` , можно переопределить, чтобы пользовательская проверка

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Проверка базы данных LocalDb удостоверений и таблиц, созданным Entity Framework

1. В **представление** меню, выберите **обозревателя серверов**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Разверните **DefaultConnection (WebFormsIdentity)**, разверните **таблиц**, щелкните правой кнопкой мыши **AspNetUsers** , а затем выберите **Показать таблицу данных**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Настройка приложения для проверки подлинности OWIN

На этом этапе только добавлена поддержка создания пользователей. Теперь мы собираемся продемонстрировать, как мы можем добавить проверку подлинности для входа в систему пользователя. ASP.NET Identity использует Microsoft OWIN по промежуточного слоя для проверки подлинности форм. Файл Cookie проверки подлинности OWIN, является файлом cookie и утверждений проверки подлинности на основе механизм, который может использоваться любой платформой, размещенных на [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) или IIS. В этой модели можно использовать те же пакеты проверки подлинности на нескольких платформах, включая ASP.NET MVC и Web Forms. Дополнительные сведения о проекта Katana и способах выполнения по промежуточного слоя. в разделе, зависит от узла [Приступая к работе с проектом Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Установить пакеты проверки подлинности в приложение

1. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**. Найдите и установите ***Microsoft.AspNet.Identity.Owin*** пакета. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Найдите и установите ***Microsoft.Owin.Host.SystemWeb*** пакета.

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** пакет содержит набор классов расширения OWIN для управления и настройки по промежуточного слоя проверки подлинности OWIN для использования процессом пакеты ASP.NET Identity Core.
    > **Microsoft.Owin.Host.SystemWeb** пакет содержит это сервер OWIN, который позволяет приложениям на базе OWIN для запуска в IIS с помощью конвейера запросов ASP.NET. Дополнительные сведения см. в разделе [по промежуточного слоя OWIN в службах IIS интегрирован конвейера](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Добавление классов конфигурации запуска и проверки подлинности OWIN

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **добавить**, а затем **Добавление нового элемента**. В диалоговом поле поиска текста, введите "*owin*«. Имя класса "*запуска*" и выберите **добавить**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. В файле Startup.cs добавьте выделенный код, показано ниже, чтобы настроить проверку подлинности файла cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Этот класс содержит `OwinStartup` атрибут для указания класса запуска OWIN. Каждое приложение OWIN имеет класс запуска, где указываются компоненты конвейера приложения. См. в разделе [определение класса запуска OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) Дополнительные сведения об этой модели.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Добавление веб-форм для регистрации и входа пользователей

1. Откройте *Register.aspx.cs* файл и добавьте приведенный ниже код какие выполняет вход пользователь при успешной регистрации.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Так как ASP.NET Identity и OWIN файл Cookie проверки подлинности на основе утверждений системы, платформы требует, чтобы разработчик приложений для создания [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) для пользователя. ClaimsIdentity имеет сведения о все утверждения для пользователя, например пользователь принадлежит к роли. На этом этапе можно также добавить дополнительные утверждения для пользователя.
    > - Можно вход пользователя с помощью AuthenticationManager из OWIN и вызывая метод `SignIn` и передавая ClaimsIdentity, как показано выше. Этот код будет вход пользователя и создать файл cookie также. Этот вызов является аналогом [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) используемые [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) модуля.
2. В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **добавить**, а затем **веб-формы**. Имя веб-формы **входа**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Замените содержимое файла *Login.aspx* файла следующим кодом:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Замените содержимое файла *Login.aspx.cs* файл со следующими:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Теперь проверяет состояние текущего пользователя и выполняет действия на основе его `Context.User.Identity.IsAuthenticated` состояния.
    >     **Отображение вход от имени пользователя** : Инфраструктуру идентификации Microsoft ASP.NET перечислил методы расширения в [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) , позволит вам получить `UserName` и `UserId` для вошедшего в систему пользователя. Эти методы расширения определяются в `Microsoft.AspNet.Identity.Core` сборки. Эти методы расширения являются заменой [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Метод входа: `This` метод заменяет предыдущий `CreateUser_Click` метод в этом примере и теперь выполняет вход пользователь после успешного создания пользователя.   
    >   Платформа Microsoft OWIN перечислил методы расширения в `System.Web.HttpContext` , позволит вам получить ссылку на `IOwinContext`. Эти методы расширения определяются в `Microsoft.Owin.Host.SystemWeb` сборки. `OwinContext` Предоставляет `IAuthenticationManager` свойство, которое представляет функциональные возможности по промежуточного слоя проверки подлинности, доступные в текущем запросе. Пользователь может войти с помощью `AuthenticationManager` из OWIN и вызвав `SignIn` и передавая `ClaimsIdentity` как показано выше. Поскольку ASP.NET Identity и OWIN файл Cookie проверки подлинности на основе утверждений система, платформа требует приложение, чтобы создать `ClaimsIdentity` для пользователя. `ClaimsIdentity` Со сведениями о все утверждения для пользователя, например пользователь принадлежит к роли. На этом этапе, этот код будет вход пользователя и создать файл cookie также можно также добавить дополнительные утверждения для пользователя. Этот вызов является аналогом [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) используемые [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) модуля.
    > - `SignOut` метод: Получает ссылку на `AuthenticationManager` из OWIN и вызывает `SignOut`. Это аналогично [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) метод, используемый [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) модуля.
5. Нажмите клавишу **Ctrl + F5** для сборки и запуска веб-приложения. Введите новое имя пользователя и пароль, а затем выберите **зарегистрировать**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Примечание. На этом этапе создается и вход нового пользователя.
6. Выберите **Выход** кнопки. Вы будете перенаправлены в журнале в форме.
7. Введите пароль или недопустимое имя пользователя и выберите **вход** кнопки. 
   `UserManager.Find` Метод вернет значение null, а сообщение об ошибке: " *Недопустимое имя пользователя или пароль* «будет отображаться.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
