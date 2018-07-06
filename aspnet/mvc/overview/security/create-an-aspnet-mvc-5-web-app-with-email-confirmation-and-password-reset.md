---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Создание безопасного веб-приложения ASP.NET MVC 5 со входом, отправить по электронной почте подтверждение и сброс пароля (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом руководстве показано, как создавать веб-приложение ASP.NET MVC 5 с подтверждение по электронной почте и сбрасывать пароль с помощью системы членства ASP.NET Identity. ЦС...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1387c3e9c03e011b610a070aa0c273ded23b463e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823551"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Создание безопасного веб-приложения ASP.NET MVC 5 со входом, отправить по электронной почте подтверждение и сброс пароля (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этом руководстве показано, как создавать веб-приложение ASP.NET MVC 5 с подтверждение по электронной почте и сбрасывать пароль с помощью системы членства ASP.NET Identity. Можно загрузить готовое приложение [здесь](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Загружаемый файл содержит отладки вспомогательные методы, которые позволяют тестировать подтверждение по электронной почте и SMS без настройки электронной почты или поставщика SMS.
> 
> Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Создание приложения ASP.NET MVC

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо установить [Visual Studio 2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для работы с этим руководством.


1. Создайте новый проект веб-ASP.NET и выберите шаблон MVC. Веб-форм также поддерживает ASP.NET Identity, поэтому можно выполнить аналогичные действия в приложении web forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Оставьте проверку подлинности по умолчанию как **учетные записи отдельных пользователей**. Если вы хотите разместить приложение в Azure, этот флажок установлен. Далее в этом руководстве описывается развертывание в Azure. Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Задайте [проект для использования SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Запустите приложение, нажмите кнопку **зарегистрировать** ссылку и регистрации пользователя. На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) атрибута.
5. В обозревателе сервера перейдите к **Connections\DefaultConnection\Tables\AspNetUsers данных**, щелкните правой кнопкой мыши и выберите **откройте определение таблицы**.

    На следующем рисунке показана `AspNetUsers` схемы:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Щелкните правой кнопкой мыши **AspNetUsers** таблицы и выберите **Показать таблицу данных**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 На этом этапе сообщение электронной почты не подтвержден.
7. Щелкните строку и выберите Удалить. Вы добавите этот адрес электронной почты еще раз на следующем шаге и отправить электронное сообщение с подтверждением.

## <a name="email-confirmation"></a>Подтверждение по электронной почте

Это рекомендуется, чтобы подтвердить регистрацию новых пользователей, чтобы проверить, они не выполняется олицетворение кто-то другой адрес электронной почты (то есть они еще не зарегистрировано другого пользователя по электронной почте). Предположим, что у вас есть дискуссионный форум, следует предотвратить `"bob@example.com"` из регистрации в качестве `"joe@contoso.com"`. Без подтверждение по электронной почте `"joe@contoso.com"` удалось получить нежелательные сообщения электронной почты из приложения. Предположим, что Боб случайно зарегистрирован как `"bib@example.com"` и не заметили, он не сможет использовать пароль восстановления, так как приложение не имеет правильное сообщение электронной почты. Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов и не предоставляет защиту от определено отправителей нежелательной почты, они имеют много псевдонимы рабочий электронной почты, которые они могут использовать для регистрации.

Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они подтверждения по электронной почте, SMS-сообщение или другой механизм. <a id="build"></a>В следующих разделах мы включим подтверждение по электронной почте и измените код, чтобы предотвратить только что зарегистрированным пользователям войти в систему, пока не будет подтверждена доступ к электронной почте.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Подключить SendGrid

Несмотря на то, что этот учебник только показано, как добавить уведомление по электронной почте через [SendGrid](http://sendgrid.com/), вы можете отправить электронную почту с помощью SMTP и другие механизмы (см. в разделе [дополнительные ресурсы](#addRes)).

1. В консоли диспетчера пакетов введите следующую команду: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Перейдите к [страницу регистрации Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) и зарегистрировать для получения бесплатной учетной записи SendGrid. Настройка SendGrid, добавив код, аналогичный следующему *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Необходимо добавить включает в себя следующее:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Для простоты в этом примере мы будем хранить параметры приложения в *web.config* файла:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде. Запись и учетные данные хранятся в параметр appSetting. В Azure, вы можете безопасно хранить эти значения на **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** вкладка на портале Azure. См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Включить подтверждение по электронной почте в контроллере Account

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Проверьте *Views\Account\ConfirmEmail.cshtml* файл имеет синтаксис правильный razor. (@ Символа в первой строке могут отсутствовать. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Запустите приложение и щелкните ссылку регистрации. После отправки формы регистрации вы вошли.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Проверьте учетную запись электронной почты и щелкните ссылку, чтобы подтвердить свой адрес электронной почты.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Требуется подтверждение по электронной почте до входа

В настоящее время в том случае, когда пользователь завершает регистрационную форму, они вошли. Обычно требуется подтвердить доступ к электронной почте перед их вход. В разделе ниже мы будем изменять код, чтобы требовать новых пользователей, иметь подтвержден по электронной почте, прежде чем они вошли (с проверкой подлинности). Обновление `HttpPost Register` метод с следующие выделенные изменения:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

При комментировании `SignInAsync` метод, пользователь не будет подписано путем регистрации. `TempData["ViewBagLink"] = callbackUrl;` Строки могут быть использованы для [отладку приложения](#dbg) и проверка регистрации без отправки по электронной почте. `ViewBag.Message` используется для отображения инструкции подтверждение. [Загрузить образец](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) содержит код для тестирования без настройки по электронной почте подтверждение по электронной почте, а также можно использовать для отладки приложения.

Создание `Views\Shared\Info.cshtml` файл и добавьте следующую разметку razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Добавить [атрибут Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) для `Contact` метод действия контроллера Home. Вы можете щелкнуть **контакт** ссылку для проверки анонимных пользователей нет доступа и прошедшие проверку подлинности пользователи имеют доступ.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Вы также должны обновить `HttpPost Login` метод действия:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Обновление *Views\Shared\Error.cshtml* представление, чтобы отобразить сообщение об ошибке:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Удалить все учетные записи в **AspNetUsers** таблица, которая содержит псевдоним электронной почты, который вы хотите протестировать. Запустите приложение и убедитесь, что не удается войти до подтверждения ваш адрес электронной почты. Когда вы подтвердите адрес электронной почты, нажмите кнопку **контакт** ссылку.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Восстановление пароля

Удалите символы комментария из `HttpPost ForgotPassword` метода действия в контроллере account:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Удалите символы комментария из `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) в *Views\Account\Login.cshtml* файл представления razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

На страницу входа, теперь будет отображать ссылку для сброса пароля.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Повторно отправить ссылку для подтверждения по электронной почте

Когда пользователь создает новую учетную запись локального, по электронной почте ссылку для подтверждения, они необходимы для использования, прежде чем они могут войти на. Если пользователь случайно удалит сообщение подтверждения или сообщение никогда не будет доставлено, им потребуется повторная отправка ссылку для подтверждения. Следующие изменения кода показано, как для этого.

Добавьте следующий вспомогательный метод в нижнюю часть *файл Controllers\AccountController.cs* файл:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Обновите метод регистрации для использования нового модуля поддержки:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Обновите метод входа для повторной отправки пароля, если учетная запись пользователя не имеет подтверждения:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Объединить учетные записи социальных сетей и локального имени входа

Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности **RickAndMSFT@gmail.com** сначала создается как локальное имя входа, но можно создать учетную запись как журнал социальных сетей в первой, а затем добавить локальное имя входа.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Щелкните **управление** ссылку. Примечание **внешних имен входа: 0** связанные с этой учетной записи.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Щелкните ссылку, чтобы другой журнал в службе и принимать запросы приложений. Были объединены две учетные записи, вы сможете выполнить вход с использованием либо учетной записи. Вы можете добавить локальные учетные записи, в случае их социальных сетей журнала в службе проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.

На следующем рисунке, Tom является журналом социальных сетей в (который также можно увидеть из **внешних имен входа: 1** на странице).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Щелкнув **выберите пароль** позволяет добавлять в локальный журнал на связанные с той же учетной записи.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Подтверждение по электронной почте Подробнее

Мои руководстве [подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) переходит в этом разделе с более подробными сведениями.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Отладка приложения

Если вы не получили сообщение электронной почты, содержащее ссылку:

- Проверьте папку нежелательной или Нежелательная почта.
- Войдите в учетную запись SendGrid и выберите [действия по электронной почте ссылку](https://sendgrid.com/logs/index).

Чтобы проверить ссылку для проверки без электронной почты, загрузите [готовый пример](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Ссылку для подтверждения и коды подтверждения будет отображаться на странице.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Рекомендуемые ресурсы по ссылкам в ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) содержит более подробные сведения на подтверждение пароля восстановления и учетную запись.
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этом руководстве показано, как писать приложения ASP.NET MVC 5 с авторизацией на Facebook и Google OAuth 2. Также показано, как добавить дополнительные данные для базы данных удостоверений.
- [Развертывание безопасного приложения ASP.NET MVC с членством, OAuth и базой данных SQL в Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). В этом руководстве добавляется развертывания Azure, как защитить приложения с помощью ролей, как использовать API членства для добавления пользователей и ролей и дополнительные функции безопасности.
- [Создание приложения Google для oauth2 и подключение приложения к проекту](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Создать приложение в Facebook и подключить приложение к проекту](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Настройка SSL в проекте](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
