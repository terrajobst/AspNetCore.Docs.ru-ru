---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Создание безопасного веб-приложения ASP.NET MVC 5 с журналом, электронной почты, пароль и подтверждение сброса (C#) | Документы Microsoft
author: Rick-Anderson
description: Этого учебника показано, как построить веб-приложение ASP.NET MVC 5 с подтверждения электронной почты и пароль, с помощью системы членства ASP.NET Identity. ЦС...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "34452572"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Создание безопасного веб-приложения ASP.NET MVC 5 с журналом, электронной почты, пароль и подтверждение сброса (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этого учебника показано, как построить веб-приложение ASP.NET MVC 5 с подтверждения электронной почты и пароль, с помощью системы членства ASP.NET Identity. Завершенное приложение можно загрузить [здесь](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Загружаемый файл содержит отладки вспомогательные методы, которые позволяют тестировать подтверждения электронной почты и SMS без настройки электронной почты или поставщика SMS.
> 
> Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Создание приложения ASP.NET MVC

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо установить [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для выполнения заданий данного учебника.


1. Создайте новый проект веб-ASP.NET и выберите шаблон MVC. Web Forms также поддерживает ASP.NET Identity, так что можно выполнить аналогичные шаги в приложении web forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Оставить как проверку подлинности по умолчанию **индивидуальные учетные записи**. Если вы хотите разместить приложение в Azure, оставьте флажок установлен. Далее в этом учебнике описывается развертывание в Azure. Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Задать [проекта для использования SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Запустите приложение, нажмите кнопку **зарегистрировать** ссылку и регистрации пользователя. На этом этапе является проверку только в электронном письме с [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) атрибута.
5. В обозревателе серверов, перейдите к **Connections\DefaultConnection\Tables\AspNetUsers данные**, щелкните правой кнопкой мыши и выберите **откройте определение таблицы**.

    На следующем рисунке показана `AspNetUsers` схемы:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Щелкните правой кнопкой мыши **AspNetUsers** таблицы и выберите **Показать таблицу данных**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 На этом этапе сообщение электронной почты не подтвержден.
7. Щелкните строку и выберите Удалить. Мы добавим это сообщение электронной почты еще раз на следующем шаге и отправить сообщение с подтверждением.

## <a name="email-confirmation"></a>Подтверждение по электронной почте

Рекомендуется для подтверждения адреса электронной почты Регистрация нового пользователя для проверки того, они не являются олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя). Предположим, что у вас есть дискуссионный форум, может потребоваться запретить `"bob@example.com"` из регистрации в качестве `"joe@contoso.com"`. Без подтверждения по электронной почте `"joe@contoso.com"` давало нежелательных электронной почты из приложения. Предположим, что Боб случайно зарегистрирован как `"bib@example.com"` и не заметили, он не сможет использовать пароль восстановления, так как приложение не имеет правильное сообщение электронной почты. Предоставляет ограниченную защиту от программы-роботы подтверждение по электронной почте и не обеспечивает защиты от нежелательных сообщений определяется у них много рабочей электронной почты псевдонимы, которые они могут использовать для регистрации.

Как правило, требуется запретить пользователям новых учетных данных для веб-узла до подтверждения по электронной почте, SMS-сообщение или другого механизма. <a id="build"></a>В следующих разделах мы включить подтверждение по электронной почте и изменить код, чтобы запретить новым зарегистрированным пользователям войти в систему, пока не будет подтверждена электронной почте.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Подключить SendGrid

Несмотря на то, что этот учебник только демонстрирует добавление уведомления по электронной почте через [SendGrid](http://sendgrid.com/), вы можете отправить по электронной почте с помощью SMTP и другие механизмы (см. [дополнительные ресурсы](#addRes)).

1. В консоли диспетчера пакетов введите следующую команду: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Последовательно выберите пункты [страницу регистрации Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) и зарегистрироваться для получения бесплатной учетной записи SendGrid. Настройте SendGrid, добавив код, аналогичный следующему *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Необходимо добавить включает в себя следующее:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Для простоты в этом примере мы будем хранить параметры приложения в *web.config* файла:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. Учетная запись и учетные данные хранятся в appSetting. В Azure, можно безопасно хранить эти значения на **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** на портале Azure. В разделе [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Включение подтверждения электронной почты в контроллера учетных записей

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Проверьте *Views\Account\ConfirmEmail.cshtml* файл имеет синтаксис razor правильно. (@-Знак в первой строке может отсутствовать. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Запустите приложение и щелкните ссылку регистрации. После отправки формы регистрации вы вошли.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Проверьте учетную запись почты и щелкните ссылку, чтобы подтвердить ваш адрес электронной почты.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Запрашивать подтверждение по электронной почте перед вход

В настоящее время после завершения форму регистрации пользователя они регистрируются в. Как правило, требуется подтверждение перед ее регистрации электронной почте. В приведенном ниже разделе нужно будет изменить требовать новых пользователей, чтобы иметь подтверждения по электронной почте, чтобы они регистрируются в (с проверкой подлинности). Обновление `HttpPost Register` метод выделенный следующие изменения:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Комментарием `SignInAsync` метод, пользователь не будет подписано путем регистрации. `TempData["ViewBagLink"] = callbackUrl;` Строки можно использовать для [выполнить отладку приложения](#dbg) и проверки регистрации без отправки по электронной почте. `ViewBag.Message` используется для отображения инструкции подтверждение. [Загрузить образец](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) содержит код для проверки по электронной почте подтверждение без настройки электронной почты, а также может использоваться для отладки приложения.

Создание `Views\Shared\Info.cshtml` и добавьте следующую разметку razor файла:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Добавить [авторизовать атрибут](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) для `Contact` метод действия контроллера Home. Можно щелкнуть **контакт** ссылку, чтобы проверить анонимных пользователей нет доступа и прошедшие проверку подлинности пользователи имеют доступ.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Необходимо также обновить `HttpPost Login` метода действия:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Обновление *Views\Shared\Error.cshtml* представление, чтобы отобразить сообщение об ошибке:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Удалить все учетные записи в **AspNetUsers** таблица, которая содержит псевдоним электронной почты, необходимо протестировать с. Запустите приложение и убедитесь, что не удается войти до подтверждения ваш адрес электронной почты. После подтверждения адреса электронной почты, нажмите кнопку **контакт** ссылку.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Восстановление пароля

Удалите символы комментария из `HttpPost ForgotPassword` метода действия в контроллера учетных записей:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Удалите символы комментария из `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) в *Views\Account\Login.cshtml* файл представления razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Страницу входа теперь будет отображать ссылку для сброса пароля.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Повторно отправить ссылку по электронной почте подтверждение

Когда пользователь создает новую локальную учетную запись, они являются уведомления по электронной почте, они должны использовать для входа ссылку для подтверждения. Если пользователь случайно удаляет сообщение электронной почты с подтверждением или сообщение никогда не будет доставлено, им потребуется повторная отправка ссылку для подтверждения. Следующие изменения кода показано, как включить эту возможность.

Добавьте следующий вспомогательный метод в нижнюю часть *Controllers\AccountController.cs* файла:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Обновите метод регистрации для использования нового модуля поддержки:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Обновите метод входа для повторной отправки пароль, если учетная запись пользователя не имеет подтверждения:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Объединение социальных сетей и локальных учетных записей

Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности **RickAndMSFT@gmail.com** сначала создается как локальное имя входа, но можно создать учетную запись в качестве социальных журналов в первой, а затем добавить локальный вход.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Щелкните **управление** ссылку. Примечание **внешних имен входа: 0** для этой учетной записи.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Щелкните ссылку, чтобы другой журнал в службе, которая принимает запросы приложения. Были объединены две учетные записи, вы сможете войти в систему с любой учетной записи. Может потребоваться пользователям добавлять локальные учетные записи, в случае их социальных вход службы проверки подлинности не работает, или что более вероятно, они утрачен доступ к учетной записи социальных сетей.

На следующем рисунке Tom является социальных вход (что можно узнать из **внешних имен входа: 1** отображаться на странице).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Щелкнув **подбора пароля** можно добавить в локальный журнал на связанные с той же учетной записи.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Подтверждение по электронной почте более глубоко

Мои учебника [подтверждение учетной записи и пароль восстановления в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) переходит в этом разделе более подробно.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Отладка приложения

Если не получить сообщение электронной почты, содержащее ссылку:

- Проверьте папку нежелательной или Нежелательная почта.
- Войдите в учетную запись SendGrid и выберите [действия электронной почты ссылка](https://sendgrid.com/logs/index).

Чтобы проверить ссылку подтверждения без электронной почты, загрузите [полного примера](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Ссылку для подтверждения и коды подтверждения отображается на странице.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Ссылки на ASP.NET Identity, рекомендуется использовать ресурсы](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Учетной записи и подтверждение пароля восстановления с ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) приведены более подробные сведения на подтверждение пароля восстановления и учетной записи.
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этого учебника показано, как написать приложение ASP.NET MVC 5 с Facebook и Google OAuth 2 авторизации. Также показано, как добавить дополнительные данные в базы данных удостоверений.
- [Развертывание приложения безопасного ASP.NET MVC с членством, OAuth и базы данных SQL в Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Этот учебник добавляет развертывания Azure, как защищать приложения с ролями, как использовать API членства для добавления пользователей и ролей и дополнительные функции безопасности.
- [Создание приложения Google OAuth 2 и подключение приложения в проект](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Создать приложение в Facebook и подключить приложения в проект](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Настройка SSL в проекте](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
