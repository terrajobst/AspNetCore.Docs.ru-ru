---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Identity (C#) | Документация Майкрософт
author: HaoK
description: Прежде чем выполнять задания этого учебника, которые нужно сначала выполнить Создание безопасного веб-приложения ASP.NET MVC 5 со входом, по электронной почте подтверждение и сброс пароля. Этот учебник...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 47dc2c1044a5964624ba2f8af4f174a2fd99d3e8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236410"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Учетная запись, пароль и Подтверждение восстановления с ASP.NET Identity (C#)

> Прежде чем выполнять задания этого учебника вы должны изучить [создать безопасное веб-приложение ASP.NET MVC 5 со входом, по электронной почте подтверждение и сброс пароля](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Это руководство содержит дополнительные сведения о нем показано, как настроить адрес электронной почты для подтверждения локальной учетной записи и разрешить пользователям Сброс забытого пароля в ASP.NET Identity.

Учетная запись локального пользователя нужно, чтобы создать пароль для учетной записи пользователя и пароль (безопасно) хранится в веб-приложения. Удостоверение ASP.NET также поддерживает учетные записи социальных сетей, которые не требуют от пользователя, создайте пароль для приложения. [Учетные записи социальных сетей](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) использовать третьих лиц (например, Google, Twitter, Facebook или Майкрософт) для аутентификации пользователей. В этом разделе объясняется следующее.

- [Создание приложения ASP.NET MVC](#createMvc) и узнать о функциональных возможностях ASP.NET Identity.
- [Построение образца удостоверений](#build)
- [Настройка подтверждение по электронной почте](#email)

Новым пользователям зарегистрировать свои псевдоним электронной почты, который создает локальную учетную запись.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Выбор кнопки "зарегистрировать" отправляет электронное сообщение с подтверждением, содержащий маркер проверки для адреса электронной почты.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Пользователю отправляется сообщение электронной почты с помощью токена подтверждения для своей учетной записи.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Выбрав ссылку подтверждает учетную запись.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Восстановление пароля

Локальных пользователей, кто забывает свой пароль может иметь маркер безопасности, отправляемые учетной записи электронной почты, что позволяет им для сброса пароля.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Вскоре, пользователю будет отправлено сообщение электронной почты со ссылкой, позволяя им для сброса пароля.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Выбрав ссылку ведущей на страницу сброса.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Выбрав **Сброс** кнопка будет подтвердить пароль был сброшен.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Создание веб-приложения ASP.NET

Начните с установки и запуска [Visual Studio 2017](https://visualstudio.microsoft.com/).


1. Создайте новый проект веб-ASP.NET и выберите шаблон MVC. Веб-форм также поддерживает ASP.NET Identity, поэтому можно выполнить аналогичные действия в приложении web forms.
2. Задайте для аутентификации **учетные записи отдельных пользователей**.
3. Запустите приложение, выберите **зарегистрировать** ссылку и регистрации пользователя. На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) атрибута.
4. В обозревателе сервера перейдите к **Connections\DefaultConnection\Tables\AspNetUsers данных**, щелкните правой кнопкой мыши и выберите **откройте определение таблицы**.

    На следующем рисунке показана `AspNetUsers` схемы:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Щелкните правой кнопкой мыши **AspNetUsers** таблицы и выберите **Показать таблицу данных**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   На этом этапе сообщение электронной почты не подтвержден.

Хранилища данных по умолчанию для ASP.NET Identity имеет Entity Framework, но можно настроить его, чтобы использовать другие хранилища данных и добавить дополнительные поля. См. в разделе [дополнительные ресурсы](#addRes) в конце этого руководства.

[Класс запуска OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) вызывается, когда приложение запускает и вызывает `ConfigureAuth` метод в *приложения\_Start\Startup.Auth.cs*, который настраивает конвейер OWIN и инициализирует ASP.NET Identity. Проверьте метод `ConfigureAuth`. Каждый `CreatePerOwinContext` вызов регистрирует обратный вызов (сохранен в `OwinContext`), будет вызываться один раз на один запрос для создания экземпляра заданного типа. Можно задать точку останова в конструкторе и `Create` метод каждого типа (`ApplicationDbContext, ApplicationUserManager`) и проверьте, они вызываются при каждом запросе. Экземпляр `ApplicationDbContext` и `ApplicationUserManager` хранится в контексте OWIN, который доступен во всем приложении. ASP.NET Identity обработчики в конвейер OWIN по промежуточного слоя. Дополнительные сведения см. в разделе [за управление жизненным циклом запроса для класса UserManager в ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

При изменении профиля безопасности, создается и сохраняется в новую метку безопасности `SecurityStamp` поле *AspNetUsers* таблицы. Обратите внимание, что `SecurityStamp` поле отличается от безопасности cookie. Файл cookie безопасности не хранится в `AspNetUsers` таблицы (или любой другой в базе данных удостоверений). Маркер безопасности cookie подписывается самостоятельно с помощью [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) и создается с `UserId, SecurityStamp` и сведения о времени истечения срока действия.

По промежуточного слоя проверяет файл cookie при каждом запросе. `SecurityStampValidator` Метод в `Startup` класс обращений к базе данных и периодически проверяет метку безопасности, указанные в `validateInterval`. Это происходит каждые 30 минут (в нашем примере) можно только после изменения профиля безопасности. Чтобы свести к минимуму количество обращений к базе данных был выбран 30-минутный интервал. См. в разделе my [руководстве двухфакторной проверки подлинности](index.md) для получения дополнительных сведений.

В комментарии в коде `UseCookieAuthentication` метод поддерживает проверку подлинности файла cookie. `SecurityStamp` Поле и с ними кода обеспечивает дополнительный уровень безопасности для приложения при изменении пароля, вы выйдете из браузера, вы вошли в систему. `SecurityStampValidator.OnValidateIdentity` Метод позволяет приложению для проверки токена безопасности, при входе пользователя, применяемое, когда вы меняете пароль или использовать внешнее имя входа. Это необходимо, чтобы убедиться, что делает недействительными все токены (файлы cookie), созданные с помощью старого пароля. В образце проекта, если вы измените пароль пользователи затем новый маркер будет создан пользователь, не делает недействительными все предыдущие токены и `SecurityStamp` обновляется.

Системы удостоверений можно настроить приложение так при изменении профиля безопасности пользователей (например, когда пользователь изменяет свой пароль или изменения связанного имени входа (например, от Facebook, Google, учетной записи Майкрософт, т. д.), пользователя из всех экземпляры браузера. Например, на рисунке ниже показано [пример единого выхода](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) приложение, которое позволяет пользователю выход из всех экземпляров браузера (в данном случае IE, Firefox и Chrome), выбрав одну кнопку. Кроме того образец позволяет только выход из экземпляра конкретного браузера.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Пример единого выхода](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) приложения показано, как ASP.NET Identity позволяет повторно создать токен безопасности. Это необходимо, чтобы убедиться, что делает недействительными все токены (файлы cookie), созданные с помощью старого пароля. Эта функция обеспечивает дополнительный уровень безопасности для приложения; При изменении пароля, вы выйдете из которой был выполнен вход в это приложение.

*Приложения\_Start\IdentityConfig.cs* файл содержит `ApplicationUserManager`, `EmailService` и `SmsService` классы. `EmailService` И `SmsService` классы, реализующие каждого `IIdentityMessageService` интерфейс, поэтому у вас есть общие методы в каждом классе, для настройки электронной почты и SMS. Несмотря на то, что этот учебник только показано, как добавить уведомление по электронной почте через [SendGrid](http://sendgrid.com/), вы можете отправить электронную почту с помощью SMTP и другие механизмы.

`Startup` Класс также содержит шаблон Добавление входа в социальные сети (Facebook, Twitter и др.), см. в разделе my руководстве [приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Дополнительные сведения.

Изучите `ApplicationUserManager` класс, который содержит сведения об удостоверениях пользователей и настраивает следующие функции:

- К надежности паролей.
- Блокировка пользователя (попытки и время).
- Двухфакторная проверка подлинности (2FA). Я расскажу об 2FA и SMS в следующем учебнике.
- Подключение службы SMS и электронной почты. (Я расскажу о SMS в другом руководстве).

`ApplicationUserManager` Класс является производным от универсального `UserManager<ApplicationUser>` класса. `ApplicationUser` является производным от [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` является производным от универсального `IdentityUser` класса:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Указанные выше свойства совпадают со свойствами в `AspNetUsers` приведенной выше таблице.

Универсальные аргументы на `IUser` позволяют создать производный класс с помощью различных типов для первичного ключа. См. в разделе [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) пример, в котором показано, как изменить первичный ключ из строки в int или GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) определяется в *Models\IdentityModels.cs* как:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Выделенный выше код создает [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity и OWIN файл Cookie проверки подлинности на основе утверждений, поэтому платформа требует приложение, чтобы создать `ClaimsIdentity` для пользователя. `ClaimsIdentity` содержит сведения о всех утверждений для пользователя, такие как имя пользователя, срок действия и какие роли он принадлежит. На этом этапе можно также добавить дополнительные утверждения для пользователя.

OWIN `AuthenticationManager.SignIn` метод передает в `ClaimsIdentity` и выполняет вход пользователя:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) показано, как можно добавить дополнительные свойства, `ApplicationUser` класса.

## <a name="email-confirmation"></a>Подтверждение по электронной почте

Рекомендуется подтвердить адрес электронной почты, регистрация нового пользователя для проверки того, они не выполняется олицетворение кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте). Предположим, что у вас есть дискуссионный форум, следует предотвратить `"bob@example.com"` из регистрации в качестве `"joe@contoso.com"`. Без подтверждение по электронной почте `"joe@contoso.com"` удалось получить нежелательные сообщения электронной почты из приложения. Предположим, что Боб случайно зарегистрирован как `"bib@example.com"` и не заметили, он не сможет использовать пароль восстановления, так как приложение не имеет правильное сообщение электронной почты. Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов и не предоставляет защиту от определено отправителей нежелательной почты, они имеют много псевдонимы рабочий электронной почты, которые они могут использовать для регистрации. В приведенном ниже примере пользователь не сможет изменить свой пароль, пока не будет подтверждена своей учетной записью (путем их выбора ссылкой для подтверждения полученных на учетную запись электронной почты, они регистрируются с.) Этот рабочий процесс можно применять для других сценариев, например отправить ссылку для подтверждения и сбросить пароль на новые учетные записи, созданные администратором, отправки сообщения электронной почты пользователя, когда они изменили свой профиль и т. д. Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они подтверждения по электронной почте, SMS-сообщение или другой механизм. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Создать более полный пример

В этом разделе будет использоваться NuGet для загрузки более полный пример, который мы будем работать с.

1. Создайте новый ***пустой*** веб-проект ASP.NET.
2. В консоли диспетчера пакетов введите следующие команды: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   В этом руководстве мы будем использовать [SendGrid](http://sendgrid.com/) для отправки электронной почты. `Identity.Samples` Пакет устанавливает код, мы будем работать с.
3. Задайте [проект для использования SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Проверка создания локальной учетной записи, запустив приложение, выбрав **зарегистрировать** связать и обратной передачи формы регистрации.
5. Выберите ссылку для электронной почты демонстрационной, которое имитирует подтверждение по электронной почте.
6. Удалите демонстрации по электронной почте ссылку подтверждения код из примера ( `ViewBag.Link` кода в контроллере account. См. в разделе `DisplayEmail` и `ForgotPasswordConfirmation` методов и представлений razor действия).

> [!WARNING]
> Если изменить какие-либо параметры безопасности в этом образце, производства приложений потребуется пройти проверку подлинности, которая явно вызывает изменения, внесенные аудита безопасности.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Изучите код в приложении\_Start\IdentityConfig.cs

Пример показано, как создать учетную запись и добавить его в *администратора* роли. Следует заменить адрес электронной почты в примере с адресом электронной почты, который будет использоваться для учетной записи администратора. Самым простым способом прямо сейчас, чтобы создать учетную запись администратора является программно с помощью `Seed` метод. Мы надеемся инструмент в будущем, позволит вам создавать и администрировать пользователей и ролей. В примере кода позволяют создавать и управлять пользователями и ролями, но сначала необходимо иметь учетную запись администраторов для выполнения роли и страниц администрирования пользователя. В этом примере учетная запись администратора создается при будет заполняться данными базы данных.

Сменить пароль и измените имя на учетную запись, где вы можете получать уведомления по электронной почте.

> [!WARNING]
> Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде.

Как упоминалось ранее, `app.CreatePerOwinContext` вызова в классе startup добавляет обратные вызовы для `Create` метод приложения базы данных содержимого, пользователя диспетчера роли диспетчера классов и. Вызывает конвейер OWIN `Create` метод эти классы для каждого запроса и сохраняет контекст для каждого класса. Контроллера учетных записей предоставляет диспетчера пользователей из контекста HTTP (который содержит контекст OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

При регистрации локальной учетной записи пользователя `HTTP Post Register` вызывается метод:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

В приведенном выше коде используются данные модели для создания учетной записи пользователя с помощью электронной почты и пароля. Если псевдоним электронной почты находится в хранилище данных, происходит сбой при создании учетной записи, и отображается форма. `GenerateEmailConfirmationTokenAsync` Метод создает маркер безопасного подтверждения и сохраняет его в хранилище данных ASP.NET Identity. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) метод создает значение типа, содержащее ссылку `UserId` и токена подтверждения. Эту ссылку, затем по электронной почте для пользователя, пользователь может выбрать ссылку в свое приложение электронной почты для активации учетной записи.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Настройка подтверждение по электронной почте

Перейдите к [страницу регистрации Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) и зарегистрируйтесь для участия в учетной записи. Добавьте код, аналогичный приведенному ниже, чтобы настроить SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Почтовые клиенты часто принимают только текстовые сообщения (HTML). Необходимо предоставить сообщение в текст или HTML. В приведенном выше примере SendGrid, это делается с помощью `myMessage.Text` и `myMessage.Html` приведенного выше кода.


Ниже показано, как отправить по электронной почте с использованием [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) класса where `message.Body` возвращает только ссылка.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде. Запись и учетные данные хранятся в параметр appSetting. В Azure, вы можете безопасно хранить эти значения на **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** вкладка на портале Azure. См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Введите свои учетные данные SendGrid, запустить приложение, регистрация с помощью псевдонима электронной почты можно выбрать ссылку подтверждение электронной почты. Чтобы узнать, как это сделать с помощью вашей [Outlook.com](http://outlook.com) учетную запись электронной почты, см. в разделе Джон Atten [ C# SMTP-конфигурация для узла SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) и его[удостоверений ASP.NET 2.0: Параметр вверх проверки учетной записи и авторизация двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) учитывает.

Когда пользователь выбирает **зарегистрировать** кнопки на свой адрес электронной почты отправляется электронное сообщение с подтверждением, содержащий токен проверки.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Пользователю отправляется сообщение электронной почты с помощью токена подтверждения для своей учетной записи.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Изучите код

В следующем примере кода показан метод `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Метод завершает работу, если адрес электронной почты пользователя не будет подтверждена. Если ошибку учтена для адреса недопустимый адрес электронной почты, пользователи-злоумышленники могут использовать эту информацию найти допустимый идентификатор пользователя (псевдонимы электронной почты) для атаки.

В следующем коде показан `ConfirmEmail` метод в контроллере, учетной записи, которое вызывается, когда пользователь выбирает ссылку для подтверждения в сообщении электронной почты, отправленных к ним:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Когда используется маркер забытого пароля, он станет недействительным. Следующие изменения кода в `Create` метод (в *приложения\_Start\IdentityConfig.cs* файл) задает маркеры истекает через 3 часа.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

В приведенном выше коде забытый пароль и подтверждение токены электронной почты истекает через 3 часа. Значение по умолчанию `TokenLifespan` — один день.

Следующий код демонстрирует метод подтверждение по электронной почте:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Чтобы сделать приложение более безопасным, ASP.NET Identity поддерживает двухфакторную проверку подлинности (2FA). См. в разделе [удостоверения ASP.NET 2.0: Настройки учетной записи проверки авторизации двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) , Джон Atten. Несмотря на то, что при сбое попытки пароль имени входа можно задать блокировку учетных записей, такой подход делает имя входа подвержена [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) блокировки. Мы рекомендуем использовать только с 2FA блокировки учетных записей.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Обзор пользовательских поставщиков хранилищ для ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) также показано, как добавить данные профиля в таблице users.
- [ASP.NET MVC и удостоверений 2.0: Представление об основах](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) , Джон Atten.
- [Введение в ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Объявление о выпуске RTM удостоверения ASP.NET 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) с Пранавом Растоги.
