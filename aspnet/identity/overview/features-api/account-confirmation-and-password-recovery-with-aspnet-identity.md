---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "Подтверждение и восстановления пароля с ASP.NET Identity (C#) учетной записи | Документы Microsoft"
author: HaoK
description: "Перед выполнением этого учебника, следует сначала выполнить создание безопасных веб-приложения ASP.NET MVC 5 с журналом в сбросить пароль и подтверждение по электронной почте. Этот учебник..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 548baaaa06980fb793c079b66b6edc34422eb579
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Подтверждение учетной записи и пароль восстановления в ASP.NET Identity (C#)
====================
по [поздравить Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Перед выполнением этого учебника вам следует изучить [создания безопасного веб-приложения ASP.NET MVC 5 с журналом в сбросить пароль и подтверждение по электронной почте](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Этот учебник содержит дополнительные сведения и будет показано, как настроить электронную почту для подтверждения локальную учетную запись и разрешить пользователям сбрасывать утраченный пароль в ASP.NET Identity. В этой статье было написано с Рик Андерсон ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), поздравить Hao и Suhas Joshi. NuGet образец был написан главным образом Hao поздравить.


Учетную запись локального пользователя требуется создать пароль для учетной записи пользователя и пароль (безопасно) хранится в веб-приложения. ASP.NET Identity также поддерживает социальных учетные записи, которые не требуют от пользователя, создайте пароль для приложения. [Учетные записи социальных](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) использования сторонних разработчиков (например, Google, Twitter, Facebook или Майкрософт) для проверки подлинности пользователей. В этом разделе объясняется следующее.

- [Создание приложения ASP.NET MVC](#createMvc) и изучение функций ASP.NET Identity.
- [Построение образца удостоверений](#build)
- [Настройка подтверждение по электронной почте](#email)

Новым пользователям зарегистрировать свои псевдоним электронной почты, который создает локальную учетную запись.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Нажав кнопку "Регистрация" отправляет сообщение с подтверждением, содержащий маркер проверки для своего адреса электронной почты.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Пользователю отправляется сообщение электронной почты с помощью токена подтверждения для своей учетной записи.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Подтверждает учетную запись, щелкнув ссылку.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Восстановление пароля

Локальные пользователи забудут пароль может иметь маркер безопасности, отправляемые свою учетную запись электронной почты, что дает им возможность сброса пароля.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Пользователь скоро получит сообщение электронной почты со ссылкой, позволяя им для сброса пароля.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Щелкнув ссылку ведет на страницу сброса.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Щелкнув **Сброс** подтвердит кнопка сброса пароля.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Создание веб-приложений ASP.NET

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка Visual Studio [2013 обновление 2](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо установить Visual Studio [2013 обновление 2](https://go.microsoft.com/fwlink/?LinkId=390521) для завершения этого учебника.


1. Создайте новый проект веб-ASP.NET и выберите шаблон MVC. Web Forms также поддерживает ASP.NET Identity, так что можно выполнить аналогичные шаги в приложении web forms.
2. Оставить как проверку подлинности по умолчанию **индивидуальные учетные записи**.
3. Запустите приложение, нажмите кнопку **зарегистрировать** ссылку и регистрации пользователя. На этом этапе является проверку только в электронном письме с [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) атрибута.
4. В обозревателе серверов, перейдите к **Connections\DefaultConnection\Tables\AspNetUsers данные**, щелкните правой кнопкой мыши и выберите **откройте определение таблицы**.

    На следующем рисунке показана `AspNetUsers` схемы:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Щелкните правой кнопкой мыши **AspNetUsers** таблицы и выберите **Показать таблицу данных**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 На этом этапе сообщение электронной почты не подтвержден.

Хранилище данных по умолчанию для ASP.NET Identity является Entity Framework, но его можно настроить для использования других хранилищ данных и добавить дополнительные поля. В разделе [дополнительные ресурсы](#addRes) в конце данного руководства.

[Класс запуска OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *файла Startup.cs* ) вызывается, когда приложение запускается и вызывает `ConfigureAuth` метод в *приложения\_Start\Startup.Auth.cs*, который настраивает конвейер OWIN и инициализирует ASP.NET Identity. Изучите `ConfigureAuth` метод. Каждый `CreatePerOwinContext` вызов регистрирует обратный вызов (сохранен в `OwinContext`), будет вызван один раз на каждый запрос для создания экземпляра заданного типа. Можно установить точку останова в конструкторе и `Create` метод каждого типа (`ApplicationDbContext, ApplicationUserManager`) и проверьте, они вызываются при каждом запросе. Экземпляр `ApplicationDbContext` и `ApplicationUserManager` хранится в контекст OWIN, который можно вызвать в приложении. ASP.NET Identity перехватчиков событий в конвейер OWIN по промежуточного слоя файлов cookie. Дополнительные сведения см. в разделе [за управление временем существования запроса для класса UserManager в ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

При изменении профиля безопасности новой метки безопасности создается и сохраняется в `SecurityStamp` поле *AspNetUsers* таблицы. Следует отметить, что `SecurityStamp` поля отличается от безопасности cookie. Файл cookie безопасности не хранятся в `AspNetUsers` таблицы (или любом другом месте в базе данных удостоверений). Маркер безопасности cookie самостоятельно подписывается с помощью [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) и создается `UserId, SecurityStamp` и сведения о времени истечения срока действия.

По промежуточного слоя файлов cookie проверяет куки-файл для каждого запроса. `SecurityStampValidator` Метод в `Startup` класс обращений к базе данных и периодически проверяет метку безопасности в соответствии с `validateInterval`. Это происходит каждые 30 минут (в нашем примере) только изменения профиля безопасности. Чтобы свести к минимуму обращений к базе данных был выбран интервал 30 минут. В разделе Мои [учебника двухфакторной проверки подлинности](index.md) для получения дополнительных сведений.

В комментарии в коде `UseCookieAuthentication` метод обеспечивает поддержку проверки подлинности файла cookie. `SecurityStamp` Поля и связанного кода обеспечивает дополнительный уровень безопасности для приложения, при изменении пароля, при подключении из браузера, с которой выполнен вход. `SecurityStampValidator.OnValidateIdentity` Метод позволяет приложению для проверки маркера безопасности при входе пользователя, применяемое, когда вы меняете пароль или использовать внешнее имя входа. Это необходимо, чтобы убедиться, что все токены (куки-файлы), созданные со старым паролем, становятся недействительными. В образце проекта при изменении пароля пользователя затем новый маркер создается для пользователя, не делает недействительными все предыдущие токены и `SecurityStamp` поле обновляется.

Система идентификации позволяют настроить приложения, при изменении профиля безопасности пользователей (например, когда пользователь изменяет свой пароль или изменения связанного имени входа (например, от Facebook, Google, учетной записи Майкрософт, т. д.), пользователь выполнил выход из всех экземпляры браузера. Например, на рисунке ниже показана [образец единого выхода](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) приложение, которое пользователь может выйти из всех экземпляров браузера (в данном случае IE, Firefox и Chrome) нажатием одной кнопки. Кроме того образец можно только выйдите из экземпляра конкретного браузера.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Образец единого выхода](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) приложение показывает, как ASP.NET Identity позволяет повторно создать токен безопасности. Это необходимо, чтобы убедиться, что все токены (куки-файлы), созданные со старым паролем, становятся недействительными. Эта функция обеспечивает дополнительный уровень безопасности для приложения; При изменении пароля, где был выполнен вход в это приложение будет выхода.

*Приложения\_Start\IdentityConfig.cs* файл содержит `ApplicationUserManager`, `EmailService` и `SmsService` классы. `EmailService` И `SmsService` классы, реализующие каждого `IIdentityMessageService` интерфейс, поэтому имеют общие методы в каждом классе для настройки электронной почты и SMS. Несмотря на то, что этот учебник только демонстрирует добавление уведомления по электронной почте через [SendGrid](http://sendgrid.com/), вы можете отправить по электронной почте с помощью SMTP и другие механизмы.

`Startup` Класс также содержит шаблон формы, добавление социальных имена входа (Facebook, Twitter, т. д.), см. Мои учебнике [приложения MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Дополнительные сведения.

Изучите `ApplicationUserManager` класс, который содержит данные удостоверений пользователей и настраивает следующие возможности:

- Требования к надежности пароля.
- Блокировка пользователя (попыток и времени).
- Двухфакторная проверка подлинности (2FA). В следующем учебнике будет рассмотрена 2FA и SMS.
- Подключении к электронной почте и службам SMS. (SMS будет рассмотрена в следующем учебнике).

`ApplicationUserManager` Класс является производным от универсального `UserManager<ApplicationUser>` класса. `ApplicationUser`является производным от [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser`является производным от универсального `IdentityUser` класса:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Указанные выше свойства совпадают со свойствами в `AspNetUsers` приведенной выше таблице.

Универсальные аргументы на `IUser` позволяют создать производный класс с помощью различных типов для первичного ключа. В разделе [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) пример, в котором показано, как изменить первичный ключ из строки в int или GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) определяется в *Models\IdentityModels.cs* как:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Выделенный код выше приводит к возникновению ошибки [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity и проверки подлинности файла Cookie OWIN на основе утверждений, поэтому платформа требует приложение, чтобы создать `ClaimsIdentity` для пользователя. `ClaimsIdentity`содержит сведения о всех утверждений для пользователя, такие как имя пользователя, срок действия и какие роли принадлежит пользователь. На этом этапе также можно добавить дополнительные утверждения для пользователя.

OWIN `AuthenticationManager.SignIn` метод передает в `ClaimsIdentity` и выполняет вход пользователя:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) показано, как можно добавить дополнительные свойства, `ApplicationUser` класса.

## <a name="email-confirmation"></a>Подтверждение по электронной почте

Рекомендуется подтверждение по электронной почте, зарегистрировать нового пользователя для проверки того, они не являются олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя). Предположим, что у вас есть дискуссионный форум, может потребоваться запретить `"bob@example.com"` из регистрации в качестве `"joe@contoso.com"`. Без подтверждения по электронной почте `"joe@contoso.com"` давало нежелательных электронной почты из приложения. Предположим, что Боб случайно зарегистрирован как `"bib@example.com"` и не заметили, он не сможет использовать пароль восстановления, так как приложение не имеет правильное сообщение электронной почты. Предоставляет ограниченную защиту от программы-роботы подтверждение по электронной почте и не обеспечивает защиты от нежелательных сообщений определяется у них много рабочей электронной почты псевдонимы, которые они могут использовать для регистрации. В следующем примере нельзя будет изменить свой пароль, пока не будет подтверждена их учетная запись пользователя (путем их щелкнуть ссылку для подтверждения получения на учетную запись электронной почты, они зарегистрированы.) Этот рабочий процесс можно применять для других сценариев, например отправить ссылку для подтверждения и сбросить пароль на новые учетные записи, созданные администратором, отправка сообщения электронной почты пользователя, если они были изменены их профиль и т. д. Как правило, требуется запретить пользователям новых учетных данных для веб-узла до подтверждения по электронной почте, SMS-сообщение или другого механизма. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Более полный пример построения

В этом разделе будет использоваться NuGet для загрузки более полный пример, который мы работаем над.

1. Создайте новый ***пустой*** веб-проекте ASP.NET.
2. В консоли диспетчера пакетов введите следующие команды: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 В этом учебнике мы будем использовать [SendGrid](http://sendgrid.com/) для отправки электронной почты. `Identity.Samples` Пакет устанавливает код, мы будем работать с.
3. Задать [проекта для использования SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Создания локальной учетной записи тестового путем запуска приложения, щелкнув **зарегистрировать** связь и передачу форму регистрации.
5. Щелкните ссылку по электронной почте Демонстрация имитирует подтверждение по электронной почте.
6. Удалите Демонстрация по электронной почте ссылку подтверждения код из примера ( `ViewBag.Link` кода в контроллера учетных записей. В разделе `DisplayEmail` и `ForgotPasswordConfirmation` методы действий и представления razor).

> [!NOTE]
> Предупреждение: При изменении параметров безопасности в этом образце производства приложения потребуется проходить аудит безопасности, которая явно вызывает изменения.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Анализ кода в приложении\_Start\IdentityConfig.cs

Пример показано, как создать учетную запись и добавить его в *администратора* роли. Следует заменить сообщения электронной почты в образце с адресом электронной почты, который будет использоваться для учетной записи администратора. Самым простым способом прямо сейчас, чтобы создать учетную запись администратора является программным способом в `Seed` метод. Мы надеемся инструмент в будущем, дает возможность создавать и администрировать пользователей и ролей. В образце кода позволяют создавать и управлять пользователями и ролями, но сначала необходимо иметь учетную запись администраторов для выполнения ролей и страниц администрирования пользователя. В этом образце учетной записи администратора создается при заполнения базы данных.

Изменить пароль и измените имя на учетную запись, позволяющие получать уведомления по электронной почте.

> [!WARNING]
> Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде.

Как упоминалось ранее, `app.CreatePerOwinContext` вызова в классе запуска добавляет обратные вызовы для `Create` метод приложения DB содержимого, пользователь manager и роль диспетчера классов. Вызывает конвейер OWIN `Create` метод эти классы для каждого запроса и сохраняет контекст для каждого класса. Контроллера учетных записей раскрывает диспетчера пользователей из контекста HTTP (который содержит контекст OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Когда пользователь регистрирует локальной учетной записи, `HTTP Post Register` вызывается метод:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Приведенный выше код использует данные модели для создания учетной записи пользователя с помощью электронной почты и пароль. Если псевдоним электронной почты находится в хранилище данных, происходит сбой создания учетной записи и отображается форма. `GenerateEmailConfirmationTokenAsync` Метод создает токен безопасности подтверждения и сохраняет его в хранилище данных ASP.NET Identity. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) метод Создание ссылки, содержащей `UserId` и токена подтверждения. Эта ссылка затем отправляется сообщение для пользователя, пользователь может щелкнуть ссылку в свое приложение электронной почты, чтобы подтвердить свою учетную запись.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Настройка подтверждение по электронной почте

Последовательно выберите пункты [страницу регистрации Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) и зарегистрировать для получения бесплатной учетной записи. Добавьте код, аналогичный приведенному ниже, чтобы настроить SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Клиенты электронной почты часто принимают только текстовые сообщения (HTML). Необходимо предоставить сообщение в текст или HTML. В приведенном выше примере SendGrid это делается с `myMessage.Text` и `myMessage.Html` приведенного выше кода.


Ниже показано, как отправить по электронной почте с помощью [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) класса where `message.Body` возвращает только по ссылке.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. Учетная запись и учетные данные хранятся в appSetting. В Azure, можно безопасно хранить эти значения на  **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  на портале Azure. В разделе [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Введите учетные данные SendGrid, запустить приложение, регистрация псевдоним электронной почты можно щелкнуть ссылку подтверждение электронной почты. Чтобы узнать, как это сделать с помощью вашей [Outlook.com](http://outlook.com) учетную запись электронной почты см. в разделе Джон Atten [конфигурацию SMTP C# для узла SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) и его[удостоверения ASP.NET 2.0: параметр вверх проверка учетной записи и авторизации двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) в блогах.

Когда пользователь щелкает **зарегистрировать** кнопку свой адрес электронной почты отправляется сообщение с подтверждением, содержащий маркер проверки.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Пользователю отправляется сообщение электронной почты с помощью токена подтверждения для своей учетной записи.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Анализ кода

В следующем примере кода показан метод `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Метод завершает работу, если адрес электронной почты пользователя еще не подтверждена. Если ошибка была отправлена для недопустимый адрес электронной почты, пользователи-злоумышленники может использовать эту информацию найти допустимый идентификатор пользователя (псевдонимы электронной почты) для атаки.

В следующем коде показано `ConfirmEmail` метод, вызываемый, когда пользователь щелкает ссылку подтверждения в сообщении электронной почты, отправленных в них контроллера учетных записей:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

После использует маркер забытый пароль становится недействительным. Следующие изменения кода в `Create` метода (в *приложения\_Start\IdentityConfig.cs* файла) задает токенов истекает через 3 часа.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

С приведенный выше код забытый пароль и подтверждение токены электронной почты будет действовать в 3 часа. Значение по умолчанию `TokenLifespan` — один день.

Следующий код демонстрирует метод подтверждение по электронной почте:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Для повышения безопасности ваше приложение, ASP.NET Identity поддерживает двухфакторную проверку подлинности (2FA). В разделе [удостоверения ASP.NET 2.0: настройки учетной записи проверки авторизации двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) , Джон Atten. Несмотря на то, что при сбое попытки пароль имени входа можно задать блокировки учетной записи, такой подход делает ваше имя входа подвержен атакам с [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) блокировки. Мы рекомендуем использовать только с 2FA блокировки учетной записи.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Обзор пользовательских поставщиков хранилищ для ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) также показано, как добавить данные профиля в таблицу пользователей.
- [ASP.NET MVC и удостоверение 2.0: представление об основах](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) , Джон Atten.
- [Введение в ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Объявляет о RTM ASP.NET удостоверения 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) по Pranav Rastogi.
