---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Создание ASP.NET Web Forms приложение с SMS двухфакторной проверки подлинности (C#) | Документы Microsoft
author: Erikre
description: Этого учебника показано, как сборка приложения веб-форм ASP.NET с помощью двухфакторной проверки подлинности. Этот учебник был разработан в дополнение к учебника под названием Cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 6c040fd3e0592b8cfd230dcd85ed3293f0a22ba7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Создание ASP.NET Web Forms приложение с SMS двухфакторной проверки подлинности (C#)
====================
по [Эрик Reitan](https://github.com/Erikre)

[Загрузить приложение ASP.NET Web Forms с помощью электронной почты и SMS двухфакторной проверки подлинности](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Этого учебника показано, как сборка приложения веб-форм ASP.NET с помощью двухфакторной проверки подлинности. Этот учебник был разработан в дополнение к учебника под названием [создания безопасного приложения веб-форм ASP.NET при регистрации пользователя, пароль и подтверждение сброса электронной почты](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Кроме того, этот учебник был основан на Рик Андерсон [MVC учебника](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Вступление

Этот учебник поможет выполнить шаги, необходимые для создания приложений веб-форм ASP.NET, поддерживающих двухфакторной проверки подлинности с помощью Visual Studio. Двухфакторная проверка подлинности — это шаг проверки подлинности дополнительных пользователей. Этот дополнительный шаг создает уникальный идентификационный номер (PIN) при входе в систему. ПИН-код обычно отправляется пользователю как электронной почты или SMS-сообщения. При авторизации в в качестве меры дополнительная проверка подлинности пользователя приложения затем вводит ПИН-код.

### <a name="tutorial-tasks-and-information"></a>Учебник задачи и сведения:

- [Создание приложения ASP.NET Web Forms](#createWebForms)
- [Настройка SMS и двухфакторная проверка подлинности](#SMS)
- [Включить двухфакторную проверку подлинности для зарегистрированного пользователя](#use2FA)
- [Дополнительные ресурсы](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Создание приложения ASP.NET Web Forms

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии также. Кроме того, необходимо создать [Twilio](https://www.twilio.com/try-twilio) учетной записи, как описано ниже.

> [!NOTE]
> Важно: Необходимо установить [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для выполнения заданий данного учебника.


1. Создайте новый проект (**файл**  - &gt; **новый проект**) и выберите **веб-приложение ASP.NET** шаблона вместе с .NET Framework версии 4.5.2 из **новый проект** диалоговое окно.
2. Из **новый проект ASP.NET** выберите **Web Forms** шаблона. Оставить как проверку подлинности по умолчанию **индивидуальные учетные записи**. Нажмите кнопку **ОК** для создания нового проекта.  
    ![Диалоговое окно Новый проект ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Включите протокол Secure Sockets Layer (SSL) для проекта. Выполните действия, доступные в **включить SSL для проекта** раздел [Приступая к работе с Web Forms учебные курсы](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. В Visual Studio откройте **консоль диспетчера пакетов** (**средства**  - &gt; **диспетчера пакетов NuGet**  - &gt; **Консоль диспетчера пакетов**) и введите следующую команду:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Настройка SMS и двухфакторная проверка подлинности

В этом учебнике используется Twilio, но можно использовать любой поставщик SMS.

1. Создание [Twilio](https://www.twilio.com/try-twilio) учетной записи.
2. Из **мониторинга** вкладка учетной записи Twilio, Копировать **ИД безопасности учетной записи** и **маркер проверки подлинности.** Вы добавите их в приложение позже.
3. Из **номера** вкладки, скопируйте вашей Twilio **номер телефона** также.
4. Сделать Twilio **ИД безопасности учетной записи**, **маркер проверки подлинности** и **номер телефона** доступны для приложения. Чтобы не усложнять вы хотите сохранить эти значения в *web.config* файла. При развертывании в Azure можно хранить значения как обеспечить безопасность при **appSettings** вкладка настройки раздел на веб-сайте. Кроме того при добавлении номер телефона, используйте только цифры.   
   Обратите внимание, что можно также добавить учетных данных SendGrid. SendGrid является службой уведомлений по электронной почте. Для сведения о включении SendGrid, обратитесь к разделу «Ловушка вверх SendGrid» учебника под названием [создавать Secure ASP.NET веб-форм приложения с Регистрация пользователя, пароль и подтверждение сброса электронной почты.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. В этом примере учетная запись и учетные данные хранятся в **appSettings** раздел *Web.config* файла. В Azure, можно безопасно хранить эти значения на **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** на портале Azure. Дополнительные сведения см. в разделе Рик Андерсон под названием [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Настройка `SmsService` класса в *приложения\_Start\IdentityConfig.cs* выделено желтым изменения файлов, сделав следующее: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Добавьте следующие `using` инструкции в начало *IdentityConfig.cs* файла: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Обновление *Account/Manage.aspx* файла путем удаления строки, выделены желтым цветом:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. В `Page_Load` обработчик *Manage.aspx.cs* кода, раскомментируйте ниже строку кода, выделены желтым цветом, чтобы он появится в виде: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. В дополнительном коде из *учетной записи*/*TwoFactorAuthenticationSignIn.aspx.cs*, обновить `Page_Load` обработчика, добавив следующий код, выделены желтым цветом: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Внося изменения кода выше, DropDownList «Поставщики», содержащий параметры проверки подлинности не будут сброшены до первого значения. Это позволит пользователю выбрать все параметры, используемые при проверке подлинности, а не только первую успешно.
10. В **обозревателе решений**, щелкните правой кнопкой мыши *Default.aspx* и выберите **задать в качестве начальной страницы**.
11. При тестировании приложения, необходимо вначале построить приложение (**Ctrl**+**Shift**+**B**), а затем запустите приложение (**F5**) и Выберите **зарегистрировать** для создания новой учетной записи пользователя или выберите **входа** Если учетная запись пользователя уже зарегистрирован.
12. После (имени пользователя) входа, щелкните идентификатор пользователя (адрес электронной почты) в панели навигации, чтобы отобразить **Управление учетной записью** страницы (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Нажмите кнопку **добавить** рядом с **номер телефона** на **Управление учетной записью** страницы.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Номер телефона (имени пользователя) место для получения SMS-сообщений (текстовых сообщений) и нажмите кнопку Добавить **отправить** кнопки.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    На этом этапе приложение будет использовать учетные данные из *Web.config* связаться Twilio. SMS-сообщения (текст сообщения) будут отправляться на телефоне, связанный с учетной записью пользователя. Можно проверить, просмотрев панель мониторинга Twilio было отправлено сообщение Twilio.
15. Через несколько секунд телефон, связанный с учетной записью пользователя получит сообщение, содержащее код проверки. Введите код проверки и нажмите клавишу **отправить**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Включить двухфакторную проверку подлинности для зарегистрированного пользователя

На этом этапе вы включили двухфакторной проверки подлинности для приложения. Пользователи могут с помощью двухфакторной проверки подлинности он может просто изменить свои параметры, с помощью пользовательского интерфейса. 

1. Как пользователь приложения можно включить двухфакторную проверку подлинности для учетной записи определенного, щелкнув идентификатор пользователя (псевдоним электронной почты) на панели навигации, чтобы отобразить **Управление учетной записью** страницы. Щелкните на **включить** ссылку, чтобы включить двухфакторную проверку подлинности для учетной записи.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Выйдите из системы, а затем войдите снова. Если вы включили функцию электронной почты, можно выбрать, SMS или по электронной почте для двухфакторной проверки подлинности. Если вы не включили по электронной почте, см. в учебнике под названием [создания безопасного ASP.NET веб-форм приложения с регистрации пользователей, подтверждение по электронной почте и сброса пароля](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. При этом отображается страница двухфакторной проверки подлинности, где можно ввести код (от SMS или по электронной почте).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Щелкнув **Запомнить этот браузер** флажок будет исключен из требуется использовать двухфакторную проверку подлинности для входа при использовании браузера и устройства, где установлен флажок. До тех пор, пока пользователи-злоумышленники не может получить доступ к устройству, включение двухфакторной проверки подлинности и щелкнув **Запомнить этот браузер** обеспечивают удобный доступ пароль один шаг, сохранив strong Защита двухфакторной проверки подлинности для всех видов доступа из ненадежных устройств. Это можно сделать на любом устройстве закрытого регулярно используемых.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Двухфакторная проверка подлинности с помощью SMS и электронной почты в ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Ссылки на ASP.NET Identity, рекомендуется использовать ресурсы](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Развертывание приложения безопасного ASP.NET Web Forms членства, OAuth и базы данных SQL на веб-сайт Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Серия учебников по веб-форм ASP.NET - добавить поставщика OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Учебник ряда веб-форм ASP.NET - включить SSL для проекта](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Подтверждение учетной записи и пароль восстановления в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Создать приложение в Facebook и подключить приложения в проект](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
