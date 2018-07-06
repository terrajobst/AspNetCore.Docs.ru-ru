---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Создание веб-ASP.NET Forms приложения с помощью SMS двухфакторной проверки подлинности (C#) | Документация Майкрософт
author: Erikre
description: Этом руководстве показано, как создать приложение веб-форм ASP.NET с двухфакторной проверки подлинности. Этот учебник был разработан в дополнение к учебника под названием Cr...
ms.author: aspnetcontent
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 16045b116ca5c797e7840f2ee5944e5f2c6282eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803553"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Создание веб-ASP.NET Forms приложения с помощью SMS двухфакторной проверки подлинности (C#)
====================
по [Erik Reitan](https://github.com/Erikre)

[Загрузка приложения ASP.NET Web Forms с помощью электронной почты и SMS двухфакторной проверки подлинности](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Этом руководстве показано, как создать приложение веб-форм ASP.NET с двухфакторной проверки подлинности. Этот учебник был разработан в дополнение к учебника под названием [Создание безопасного приложения веб-форм ASP.NET с регистрацией пользователей, отправить по электронной почте подтверждение и сброс пароля](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Кроме того, этот учебник был основан на Рик Андерсон [руководство по MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Вступление

Этот учебник поможет выполнить шаги, необходимые для создания приложения веб-форм ASP.NET, которое поддерживает двухфакторную проверку подлинности с помощью Visual Studio. Двухфакторная проверка подлинности — это шаг проверки подлинности дополнительных пользователей. Это дополнительное действие создает уникальный идентификационный номер (ПИН-код) при входе в систему. ПИН-код обычно отправляется пользователю как электронной почты или SMS-сообщения. Приложения здесь пользователь вводит ПИН-код в качестве меры дополнительную проверку подлинности при входе в систему.

### <a name="tutorial-tasks-and-information"></a>Учебника по задачам и сведениям:

- [Создание приложения ASP.NET Web Forms](#createWebForms)
- [Настройка SMS и двухфакторная проверка подлинности](#SMS)
- [Включить двухфакторную проверку подлинности для зарегистрированного пользователя](#use2FA)
- [Дополнительные ресурсы](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Создание приложения ASP.NET Web Forms

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии также. Кроме того, необходимо создать [Twilio](https://www.twilio.com/try-twilio) учетной записи, как описано ниже.

> [!NOTE]
> Важно: Необходимо установить [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для работы с этим руководством.


1. Создайте новый проект (**файл**  - &gt; **новый проект**) и выберите **веб-приложение ASP.NET** шаблон вместе с .NET Framework версии 4.5.2 из **новый проект** диалоговое окно.
2. Из **новый проект ASP.NET** выберите **веб-форм** шаблона. Оставьте проверку подлинности по умолчанию как **учетные записи отдельных пользователей**. Щелкните **ОК** для создания нового проекта.  
    ![Диалоговое окно "новый проект ASP.NET"](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Включите протокол Secure Sockets Layer (SSL) для проекта. Выполните действия, доступные в **включить SSL для проекта** раздел [Приступая к работе с веб-форм серии руководств](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. В Visual Studio откройте **консоль диспетчера пакетов** (**средства**  - &gt; **диспетчера пакетов NuGet**  - &gt; **Консоль диспетчера пакетов**) и введите следующую команду:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Настройка SMS и двухфакторная проверка подлинности

В этом руководстве используется Twilio, но вы можете использовать любой поставщик SMS.

1. Создание [Twilio](https://www.twilio.com/try-twilio) учетной записи.
2. Из **панели мониторинга** вкладке учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности.** Вы добавите их в свое приложение более поздней версии.
3. Из **номера** вкладке, скопируйте twilio и значениями **номер телефона** также.
4. Осуществление Twilio **ИД безопасности учетной записи**, **маркер проверки подлинности** и **номер телефона** доступны приложению. Чтобы не усложнять сохранит эти значения в *web.config* файл. При развертывании в Azure, можно хранить значения безопасно в **appSettings** вкладку "Настройка" разделе на веб-сайте. Кроме того при добавлении номер телефона, используйте только цифры.   
   Обратите внимание на то, что можно также добавить учетные данные SendGrid. SendGrid — это служба уведомлений по электронной почте. Сведения о том, как включить SendGrid, см. в разделе «Ловушка вверх SendGrid» под названием учебника [Создание безопасного веб-форм приложения ASP.NET с регистрацией пользователей, отправить по электронной почте подтверждение и сброс пароля.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде. В этом примере учетная запись и учетные данные хранятся в **appSettings** раздел *Web.config* файл. В Azure, вы можете безопасно хранить эти значения на **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** вкладка на портале Azure. Дополнительные сведения см. в разделе Рик Андерсон под названием [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Настройка `SmsService` в класс *приложения\_Start\IdentityConfig.cs* выделено желтым изменении файла, сделав следующее: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Добавьте следующий `using` инструкции в начало *IdentityConfig.cs* файла: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Обновление *Account/Manage.aspx* файла путем удаления строки выделены желтым цветом:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. В `Page_Load` обработчик *Manage.aspx.cs* кода, раскомментируйте ниже строку кода, выделены желтым цветом, чтобы он располагался как: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. В выделенном коде из *учетной записи*/*TwoFactorAuthenticationSignIn.aspx.cs*, обновить `Page_Load` обработчик, добавив следующий код, выделены желтым цветом: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Выполняя код, представленный выше изменение, DropDownList «Поставщики», содержащий параметры проверки подлинности не будут сброшены к первому значению. Это позволит пользователю успешно выбрать все параметры, используемые при проверке подлинности, а не только первую.
10. В **обозревателе решений**, щелкните правой кнопкой мыши *Default.aspx* и выберите **задать в качестве начальной страницы**.
11. При тестировании приложения, сначала выполните сборку приложения (**Ctrl**+**Shift**+**B**) и затем запустите приложение (**F5**) и Выберите **зарегистрировать** для создания учетной записи пользователя или выберите **вход** Если учетная запись пользователя уже зарегистрирован.
12. После (имени пользователя) после входа, щелкните идентификатор пользователя (адрес электронной почты) на панели навигации, чтобы отобразить **Управление учетной записью** страницы (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Нажмите кнопку **добавить** рядом с полем **номер телефона** на **Управление учетной записью** страницы.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Добавить номер телефона (имени пользователя) место для получения сообщений SMS (текстовые сообщения) и нажмите кнопку **отправить** кнопки.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    На этом этапе приложение будет использовать учетные данные с *Web.config* для связи с Twilio. SMS-сообщения (SMS) будут отправляться на телефоне, связанный с учетной записью. Вы можете проверить, что Twilio сообщения, просмотрев панель мониторинга Twilio.
15. Через несколько секунд телефона, связанный с учетной записью, получит сообщение, содержащее код проверки. Введите код проверки и нажмите клавишу **отправить**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Включить двухфакторную проверку подлинности для зарегистрированного пользователя

На этом этапе вы включили двухфакторной проверки подлинности для приложения. Для пользователя, использовать двухфакторную проверку подлинности они просто могут изменять параметры с помощью пользовательского интерфейса. 

1. Как пользователь приложения вы можете включить двухфакторную проверку подлинности для вашей конкретной учетной записи, щелкнув идентификатор пользователя (псевдоним электронной почты) на панели навигации, чтобы отобразить **Управление учетной записью** страницы. Затем щелкните **включить** ссылку, чтобы включить двухфакторную проверку подлинности для учетной записи.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Выйдите из системы, а затем войдите снова. Если вы включили функцию электронной почты, можно выбрать, SMS или электронной почты для двухфакторной проверки подлинности. Если вы не включили электронной почты, см. в руководстве под названием [Создание безопасного веб-форм приложения ASP.NET с помощью регистрации пользователей, подтверждение по электронной почте и сброса пароля](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. При этом отображается страница двухфакторной проверки подлинности, где можно ввести код (от SMS или электронной почты).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Щелкнув **запомнить браузер** "флажок" исключить вас от необходимости использовать двухфакторную проверку подлинности для входа при использовании браузеров и устройств, где установлен флажок. До тех пор, пока пользователи-злоумышленники не может получить доступ к устройству, включение двухфакторной проверки подлинности и щелкнув **запомнить браузер** обеспечит удобный доступ пароль один шаг, не теряя strong Защита двухфакторной проверки подлинности для всех операций доступа с устройств, не заслуживающих доверия. Это можно сделать на любом устройстве закрытого, которые регулярно использовать.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Двухфакторная проверка подлинности с помощью SMS и электронной почты в ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Рекомендуемые ресурсы по ссылкам в ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Развертывание безопасного веб-форм приложение ASP.NET с членством, OAuth и базой данных SQL на веб-сайте Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Серии руководств веб-форм ASP.NET - Добавление поставщика OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms серии руководств — включить SSL для проекта](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Создать приложение в Facebook и подключить приложение к проекту](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
