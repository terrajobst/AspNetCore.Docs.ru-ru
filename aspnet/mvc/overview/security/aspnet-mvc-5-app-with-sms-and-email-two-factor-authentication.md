---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Приложения ASP.NET MVC 5 с помощью SMS и двухфакторная проверка подлинности электронной почты | Документы Microsoft
author: Rick-Anderson
description: Этого учебника показано, как построить веб-приложение ASP.NET MVC 5 с двухфакторной проверки подлинности. Необходимо выполнить создание безопасных ASP.NET MVC 5 веб-приложения с...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этого учебника показано, как построить веб-приложение ASP.NET MVC 5 с двухфакторной проверки подлинности. Следует выполнить [создания безопасного веб-приложения ASP.NET MVC 5 с журналом в сбросить пароль и подтверждение по электронной почте](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) перед продолжением. Завершенное приложение можно загрузить [здесь](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Загружаемый файл содержит отладки вспомогательные методы, которые позволяют тестировать подтверждения электронной почты и SMS без настройки электронной почты или поставщика SMS.
> 
> Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Создание приложения ASP.NET MVC](#createMvc)
- [Настройка SMS для двухфакторной проверки подлинности](#SMS)
- [Включение двухфакторной проверки подлинности](#enable2)
- [Дополнительные ресурсы](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Создание приложения ASP.NET MVC

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо выполнить [создания безопасного веб-приложения ASP.NET MVC 5 с журналом в сбросить пароль и подтверждение по электронной почте](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) перед продолжением. Необходимо установить [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для выполнения заданий данного учебника.


1. Создайте новый проект веб-ASP.NET и выберите шаблон MVC. Web Forms также поддерживает ASP.NET Identity, так что можно выполнить аналогичные шаги в приложении web forms.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Оставить как проверку подлинности по умолчанию **индивидуальные учетные записи**. Если вы хотите разместить приложение в Azure, оставьте флажок установлен. Далее в этом учебнике описывается развертывание в Azure. Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Задать [проекта для использования SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Настройка SMS для двухфакторной проверки подлинности

Этот учебник содержит инструкции по использованию Twilio или ASPSMS, но можно использовать любой другой поставщик SMS.

1. **Создание учетной записи пользователя с помощью поставщика SMS**  
  
   Создание [Twilio](https://www.twilio.com/try-twilio) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) учетной записи.
2. **Установка дополнительных пакетов или Добавление ссылки на службу**  
  
   Twilio:  
   В консоли диспетчера пакетов введите следующую команду:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Следующие ссылки на службу необходимо добавить:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Адрес:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Пространство имен:  
    `ASPSMSX2`
3. **Изучив учетные данные пользователя поставщика SMS**  
  
   Twilio:  
   Из **мониторинга** вкладка учетной записи Twilio, Копировать **ИД безопасности учетной записи** и **маркер проверки подлинности**.  
  
   ASPSMS:  
   Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с самостоятельно определенных **пароль**.  
  
   Позже он будет храниться эти значения в *web.config* файла в ключи `"SMSAccountIdentification"` и `"SMSAccountPassword"` .
4. **Указав идентификатор отправителя / инициатора**  
  
   Twilio:  
   Из **номера** вкладки, скопируйте Twilio номер телефона.  
  
   ASPSMS:  
   В пределах **разблокировать инициаторы** меню разблокирования одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).  
  
   Позже он будет храниться в это значение *web.config* файла в ключе `"SMSAccountFrom"` .
5. **Передача учетных данных поставщика SMS в приложение**  
  
   Предоставления учетных данных и номер телефона отправителя для приложения. Чтобы не усложнять будут храниться эти значения в *web.config* файла. При развертывании в Azure, мы может хранить значения как обеспечить безопасность при **параметры приложения** вкладка настройки раздел на веб-сайте. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. Учетная запись и учетные данные добавляются выше, чтобы не усложнять в образце кода. В разделе [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Реализация передачи данных для поставщика SMS**  
  
   Настройка `SmsService` класса в *приложения\_Start\IdentityConfig.cs* файла.  
  
   В зависимости от используемых поставщика SMS активировать **Twilio** или **ASPSMS** раздела: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Обновление *Views\Manage\Index.cshtml* представления Razor: (Примечание: не просто удалить все комментарии в существующий код, используйте приведенный ниже код.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Проверьте `EnableTwoFactorAuthentication` и `DisableTwoFactorAuthentication` методы действий в `ManageController` имеют[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) атрибута:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Запустите приложение и войти с использованием учетной записи, которую вы ранее зарегистрированного.
10. Щелкните идентификатор пользователя, который активирует `Index` метода действия в `Manage` контроллера.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Нажмите кнопку Добавить.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Метод действия отображает диалоговое окно, введите номер телефона, который может получить SMS-сообщений.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Через несколько секунд вы получите SMS с кодом проверки. Введите его и нажмите клавишу **отправить**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Представления управление показывает, что был добавлен в ваш номер телефона.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Включение двухфакторной проверки подлинности

В приложении создается шаблон необходимо включить двухфакторную проверку подлинности (2FA) с помощью пользовательского интерфейса. Чтобы включить 2FA, щелкните свой идентификатор пользователя (псевдоним электронной почты) на панели навигации.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Щелкните Включить 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Выхода из системы, затем войдите снова. Если вы включили функцию электронной почты (см. Мои [с предыдущим учебником](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), можно выбрать SMS или по электронной почте для 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Проверьте кодовая страница отображается, где можно ввести код (от SMS или по электронной почте).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA для входа при использовании браузера и устройства, где установлен флажок. До тех пор, пока пользователи-злоумышленники не может получить доступ к устройству, включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают удобный доступ пароль один шаг, сохранив строгого 2FA защиты для всех видов доступа из ненадежных устройства. Это можно сделать на любом устройстве закрытого регулярно используемых.

Этот учебник содержит краткие сведения о включении 2FA на новое приложение ASP.NET MVC. Мои учебника [двухфакторной проверки подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) попадают в подробности кода программной части образца.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) переходит в сведения о двухфакторной проверки подлинности
- [Ссылки на ASP.NET Identity, рекомендуется использовать ресурсы](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Учетной записи и подтверждение пароля восстановления с ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) приведены более подробные сведения на подтверждение пароля восстановления и учетной записи.
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этого учебника показано, как написать приложение ASP.NET MVC 5 с Facebook и Google OAuth 2 авторизации. Также показано, как добавить дополнительные данные в базы данных удостоверений.
- [Развертывание приложения безопасного ASP.NET MVC с членством, OAuth и базы данных SQL Azure веб-](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Этот учебник добавляет развертывания Azure, как защищать приложения с ролями, как использовать API членства для добавления пользователей и ролей и дополнительные функции безопасности.
- [Создание приложения Google OAuth 2 и подключение приложения в проект](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Создать приложение в Facebook и подключить приложения в проект](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Настройка SSL в проекте](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
