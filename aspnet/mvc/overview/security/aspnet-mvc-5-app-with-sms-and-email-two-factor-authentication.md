---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности | Документация Майкрософт
author: Rick-Anderson
description: Этом руководстве показано, как создавать веб-приложение ASP.NET MVC 5 с двухфакторной проверки подлинности. Следует выполнить Создание безопасного ASP.NET MVC 5 веб-приложения с помощью...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 97b73c5ae18a528d33b44a4de4afc434ac46af48
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577604"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> Этом руководстве показано, как создавать веб-приложение ASP.NET MVC 5 с двухфакторной проверки подлинности. Следует выполнить [создать безопасное веб-приложение ASP.NET MVC 5 со входом, по электронной почте подтверждение и сброс пароля](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) прежде чем продолжить. Можно загрузить готовое приложение [здесь](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Загружаемый файл содержит отладки вспомогательные методы, которые позволяют тестировать подтверждение по электронной почте и SMS без настройки электронной почты или поставщика SMS.
> 
> Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Создание приложения ASP.NET MVC](#createMvc)
- [Настройка SMS для двухфакторной проверки подлинности](#SMS)
- [Включение двухфакторной проверки подлинности](#enable2)
- [Дополнительные ресурсы](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Создание приложения ASP.NET MVC

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо выполнить [создать безопасное веб-приложение ASP.NET MVC 5 со входом, по электронной почте подтверждение и сброс пароля](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) прежде чем продолжить. Необходимо установить [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для работы с этим руководством.


1. Создайте новый проект веб-ASP.NET и выберите шаблон MVC. Веб-форм также поддерживает ASP.NET Identity, поэтому можно выполнить аналогичные действия в приложении web forms.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Оставьте проверку подлинности по умолчанию как **учетные записи отдельных пользователей**. Если вы хотите разместить приложение в Azure, этот флажок установлен. Далее в этом руководстве описывается развертывание в Azure. Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Задайте [проект для использования SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Настройка SMS для двухфакторной проверки подлинности

Этот учебник содержит инструкции по использованию Twilio или ASPSMS, но вы можете использовать любой другой поставщик SMS.

1. **Создание учетной записи пользователя с помощью поставщика SMS**  
  
   Создание [Twilio](https://www.twilio.com/try-twilio) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) учетной записи.
2. **Установка дополнительных пакетов или добавление ссылок на службы**  
  
   Twilio:  
   В консоли диспетчера пакетов введите следующую команду:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Следующие ссылки на службу нужно добавить:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Адрес:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Пространство имен:  
    `ASPSMSX2`
3. **Выяснение учетные данные поставщика SMS**  
  
   Twilio:  
   Из **панели мониторинга** вкладке учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.  
  
   ASPSMS:  
   Параметры учетной записи, перейдите к **Userkey** и скопируйте его вместе с вашей самоопределяющегося **пароль**.  
  
   Позже мы сохраним эти значения в *web.config* файла в ключи `"SMSAccountIdentification"` и `"SMSAccountPassword"` .
4. **Указав идентификатор SenderID / инициатора**  
  
   Twilio:  
   Из **номера** вкладке, скопируйте свой номер телефона Twilio.  
  
   ASPSMS:  
   В рамках **разблокировать инициаторы** меню разблокировать один или несколько авторства или выберите инициатор буквенно-цифровых (не поддерживаются все сети).  
  
   Позже мы Сохраним это значение в *web.config* файла раздел `"SMSAccountFrom"` .
5. **Передача учетных данных поставщика SMS в приложение**  
  
   Сделать доступными для приложения учетные данные и номер телефона отправителя. Для простоты мы сохраним эти значения в *web.config* файл. При развертывании в Azure, мы может хранить значения как обеспечить безопасность при **параметры приложения** вкладку "Настройка" разделе на веб-сайте. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде. Запись и учетные данные добавляются в код выше для простоты в примере. См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Реализация передачи данных для поставщика SMS**  
  
   Настройка `SmsService` в класс *приложения\_Start\IdentityConfig.cs* файла.  
  
   В зависимости от используемых поставщика SMS активировать **Twilio** или **ASPSMS** раздел: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Обновление *Views\Manage\Index.cshtml* представления Razor: (Примечание: не просто удалить все комментарии в существующий код, используйте приведенный ниже код.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Проверьте `EnableTwoFactorAuthentication` и `DisableTwoFactorAuthentication` методы действий в `ManageController` имеют[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) атрибут:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Запустите приложение и войдите с помощью учетной записи, которую вы ранее зарегистрировали.
10. Щелкните свой идентификатор пользователя, который активирует `Index` метода действия в `Manage` контроллера.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Нажмите кнопку Добавить.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Метод действия отображает диалоговое окно, введите номер телефона, который может получать SMS-сообщения.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Через несколько секунд вы получите текстовое сообщение с кодом проверки. Введите его и нажмите клавишу **отправить**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Представления управление показывает, что был добавлен ваш номер телефона.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Включение двухфакторной проверки подлинности

В шаблон, создаваемый в приложении необходимо включить двухфакторную проверку подлинности (2FA) с помощью пользовательского интерфейса. Чтобы включить 2FA, щелкните свой идентификатор пользователя (псевдоним электронной почты) на панели навигации.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Щелкните Включить 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Выход из системы, затем войдите снова. Если вы включили функцию по электронной почте (см. в разделе my [предыдущем учебном курсе](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), можно выбрать SMS или электронной почты для 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Проверьте кодовая страница отображается, где можно ввести код (от SMS или электронной почты).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Щелкнув **запомнить браузер** "флажок" исключить вас от необходимости использовать 2FA для входа при использовании браузеров и устройств, где установлен флажок. До тех пор, пока пользователи-злоумышленники не может получить доступ к устройству, позволяя 2FA и щелкнув **запомнить браузер** обеспечит удобный доступ пароль один шаг, не теряя защиты строгого 2FA для всех операций доступа с устройств, не заслуживающих доверия. Это можно сделать на любом устройстве закрытого, которые регулярно использовать.

Этот учебник содержит краткие сведения о включении 2FA на новое приложение ASP.NET MVC. Мои руководстве [двухфакторной проверки подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) подробно кода программной части примера.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) содержит подробные сведения о двухфакторной проверки подлинности
- [Рекомендуемые ресурсы по ссылкам в ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) содержит более подробные сведения на подтверждение пароля восстановления и учетную запись.
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этом руководстве показано, как писать приложения ASP.NET MVC 5 с авторизацией на Facebook и Google OAuth 2. Также показано, как добавить дополнительные данные для базы данных удостоверений.
- [Развертывание безопасного приложения ASP.NET MVC с членством, OAuth и базой данных SQL Azure веб-](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). В этом руководстве добавляется развертывания Azure, как защитить приложения с помощью ролей, как использовать API членства для добавления пользователей и ролей и дополнительные функции безопасности.
- [Создание приложения Google для oauth2 и подключение приложения к проекту](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Создать приложение в Facebook и подключить приложение к проекту](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Настройка SSL в проекте](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
