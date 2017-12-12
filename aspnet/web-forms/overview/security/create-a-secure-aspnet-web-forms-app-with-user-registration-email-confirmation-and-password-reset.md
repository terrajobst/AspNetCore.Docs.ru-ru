---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: "Создание безопасных приложений веб-форм ASP.NET при регистрации пользователя электронной почты для сброса пароля и подтверждение (C#) | Документы Microsoft"
author: Erikre
description: "Этого учебника показано, как сборка приложения веб-форм ASP.NET с помощью регистрации пользователей, подтверждение по электронной почте и пароль, с помощью члена удостоверения ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: b6f3821a8022daa26f5efcc009ab3e6283a76a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Создание безопасных приложений веб-форм ASP.NET при регистрации пользователя электронной почты для сброса пароля и подтверждение (C#)
====================
По [Эрик Reitan](https://github.com/Erikre)

> Этого учебника показано, как сборка приложения веб-форм ASP.NET с помощью регистрации пользователей, подтверждения электронной почты и пароль, с помощью системы членства ASP.NET Identity. Этот учебник был основан на Рик Андерсон [MVC учебника](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Вступление

Этот учебник поможет выполнить шаги, необходимые для создания приложения веб-форм ASP.NET с помощью Visual Studio и ASP.NET 4.5 для создания безопасных приложений веб-форм с регистрации пользователей, электронной почты, пароль и подтверждение сброса.

### <a name="tutorial-tasks-and-information"></a>Учебник задачи и сведения:

- [Создание ASP.NET Web Forms приложения](#createWebForms)
- [Подключить SendGrid](#SG)
- [Запрашивать подтверждение по электронной почте перед вход](#require)
- [Пароль восстановления и сброса](#reset)
- [Повторно отправить ссылку по электронной почте подтверждение](#rsend)
- [Устранение неполадок приложения](#dbg)
- [Дополнительные ресурсы](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Создание приложения ASP.NET Web Forms

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии также.

> [!NOTE]
> Предупреждение: Необходимо установить [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) или более поздней версии для выполнения заданий данного учебника.


1. Создайте новый проект (**файл**  - &gt; **новый проект**) и выберите **веб-приложение ASP.NET** шаблона и последнюю версию .NET Framework версии из **новый проект** диалоговое окно.
2. Из **новый проект ASP.NET** выберите **Web Forms** шаблона. Оставить как проверку подлинности по умолчанию **индивидуальные учетные записи**. Если вы хотите разместить приложение в Azure, оставьте **узел в облаке** флажков.   
 Нажмите кнопку **ОК** для создания нового проекта.  
    ![Диалоговое окно Новый проект ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Включите протокол Secure Sockets Layer (SSL) для проекта. Выполните действия, доступные в **включить SSL для проекта** раздел [Приступая к работе с Web Forms учебные курсы](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Запустите приложение, нажмите кнопку **зарегистрировать** ссылку и регистрации нового пользователя. На этом этапе на основе проверку только в электронном письме [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) атрибута, чтобы гарантировать правильность адреса электронной почты. Код для добавления подтверждение по электронной почте будет изменен. Закройте окно браузера.
5. В **обозревателя серверов** из Visual Studio (**представление**  - &gt; **обозревателя серверов**), перейдите к **Connections\ данных DefaultConnection\Tables\AspNetUsers**, щелкните правой кнопкой мыши и выберите **откройте определение таблицы**. 

    На следующем рисунке показана `AspNetUsers` схема таблицы:

    ![Схема таблицы AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. В **обозревателя серверов**, щелкните правой кнопкой мыши **AspNetUsers** таблицы и выберите **Показать таблицу данных**.  
  
    ![AspNetUsers таблицы данных](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 На этом этапе сообщение электронной почты для зарегистрированного пользователя не подтвержден.
7. Выберите в строке и выберите delete для удаления пользователя. Мы добавим это сообщение электронной почты еще раз на следующем шаге и отправить сообщение с подтверждением на адрес электронной почты.

## <a name="email-confirmation"></a>Подтверждение по электронной почте

Рекомендуется для подтверждения сообщения электронной почты во время регистрации нового пользователя для проверки того, они не являются олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя). Предположим, что у вас есть дискуссионный форум, может потребоваться запретить `"bob@cpandl.com"` из регистрации в качестве `"joe@contoso.com"`. Без подтверждения по электронной почте `"joe@contoso.com"` давало нежелательных электронной почты из приложения. Предположим, что Боб случайно зарегистрирован как `"bib@cpandl.com"` и не заметили, он не сможет использовать пароль восстановления, так как приложение не имеет правильное сообщение электронной почты. Предоставляет ограниченную защиту от программы-роботы подтверждение по электронной почте и не обеспечивает защиты от нежелательных сообщений определяется.

Как правило, требуется запретить пользователям новых учетных данных для веб-сайта, прежде чем они подтверждено либо по электронной почте, SMS-сообщение или другой механизм. В следующих разделах мы включить подтверждение по электронной почте и изменить код, чтобы запретить новым зарегистрированным пользователям войти в систему, пока не будет подтверждена электронной почте. В этом учебнике будет использоваться служба электронной почты SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Подключить SendGrid

Несмотря на то, что этот учебник только демонстрирует добавление уведомления по электронной почте через [SendGrid](http://sendgrid.com/), вы можете отправить по электронной почте с помощью SMTP и другие механизмы (см. [дополнительные ресурсы](#addRes)).

1. В Visual Studio откройте **консоль диспетчера пакетов** (**средства**  - &gt; **диспетчера пакетов NuGet**  - &gt; **Консоль диспетчера пакетов**) и введите следующую команду:  
    `Install-Package SendGrid`
2. Последовательно выберите пункты [страницу регистрации Azure SendGrid](https://azure.microsoft.com/en-us/gallery/store/sendgrid/sendgrid-azure/) и зарегистрировать для получения бесплатной учетной записи SendGrid. Вы можете также регистрации для бесплатной учетной записи SendGrid с непосредственно на [SendGrid на сайте](http://www.sendgrid.com).
3. Из **обозревателе решений** откройте *IdentityConfig.cs* файла в *приложения\_запустить* папки и добавьте следующий код, выделены желтым цветом, позволяя `EmailService` класса, чтобы настроить **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Кроме того, добавьте следующий `using` инструкции в начало *IdentityConfig.cs* файла: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Для простоты в этом примере будут храниться значения учетной записи службы электронной почты в `appSettings` раздел *web.config* файла. Добавьте следующий код XML, выделены желтым цветом, позволяя *web.config* файл в корне проекта:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. В этом примере учетная запись и учетные данные хранятся в **appSetting** раздел *Web.config* файла. В Azure, можно безопасно хранить эти значения на  **[Настройка](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  на портале Azure. Дополнительные сведения см. разделе Рик Андерсон под названием [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Добавьте значения службы электронной почты, чтобы отображать значения проверки подлинности SendGrid (имя пользователя и пароль), чтобы пользователь мог успешно отправлять электронную почту из приложения. Обязательно используйте имя учетной записи SendGrid, а не адрес электронной почты, предоставленный SendGrid.

### <a name="enable-email-confirmation"></a>Включение подтверждения по электронной почте

 Чтобы включить подтверждение по электронной почте, будет изменена кода регистрации, выполнив следующие действия.  
 

1. В *учетной записи* откройте *Register.aspx.cs* кода и обновить `CreateUser_Click` способ включения выделенные следующие изменения: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. В **обозревателе решений**, щелкните правой кнопкой мыши *Default.aspx* и выберите **задать в качестве начальной страницы**.
3. Запустите приложение, нажав клавишу **F5.** Эта страница отображается, нажмите кнопку **зарегистрировать** ссылку, чтобы отобразить страницу регистрации.
4. Введите имя электронной почты и пароль, а затем нажмите кнопку **зарегистрировать** кнопку, чтобы отправить сообщение электронной почты через SendGrid.  
 Текущее состояние проекта и кода позволит пользователю входить в систему после их заполните форму регистрации, даже если они еще не подтверждена свою учетную запись.
5. Проверьте учетную запись почты и щелкните ссылку, чтобы подтвердить ваш адрес электронной почты.  
 После отправки формы регистрации будет регистрироваться.  
    ![Пример веб-сайта — вход](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Запрашивать подтверждение по электронной почте перед вход

Несмотря на то, что вы убедились учетной записи электронной почты, на этом этапе не потребуется щелкнуть ссылку, содержащихся в сообщении проверки, чтобы быть полностью вошедшего в систему. В следующем разделе будет изменить код, требующие новых пользователей, чтобы иметь подтверждения по электронной почте, чтобы они регистрируются в (с проверкой подлинности).

1. В **обозревателе решений** Visual Studio, обновление `CreateUser_Click` события в *Register.aspx.cs* кода программной части, содержащиеся в *учетные записи* папка с Выделенная следующие изменения: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Обновление `LogIn` метод в *Login.aspx.cs* кода выделенный следующие изменения: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Запуск приложения

 Теперь, когда реализован код проверки, подтвержден ли адрес электронной почты пользователя, можно проверить функцию на обоих **зарегистрировать** и **входа** страниц. 

1. Удалить все учетные записи в **AspNetUsers** таблица, которая содержит псевдоним электронной почты, необходимо протестировать.
2. Запустите приложение (**F5**) и проверьте, нельзя зарегистрировать имени пользователя, пока вы подтвердили свой адрес электронной почты.
3. Перед подтверждением вашей новой учетной записи по электронной почте, была недавно отправлена, попробуйте войти с использованием новой учетной записи.  
 Вы увидите, что не удается выполнить вход и необходимо наличие учетной записи электронной почты подтверждена.
4. После подтверждения адреса электронной почты, выполните вход в приложение.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Пароль восстановления и сброса

1. В Visual Studio, удалите символы комментария из `Forgot` метод в *Forgot.aspx.cs* кода программной части, содержащиеся в *учетной записи* папки, чтобы метод отображается в виде ниже: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Откройте *Login.aspx* страницы. Замените существующую разметку в конец **loginForm** статьи, как показано ниже: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Откройте *Login.aspx.cs* кода и раскомментируйте следующую строку кода, выделяются желтым из `Page_Load` обработчик событий: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Запустите приложение, нажав клавишу **F5.** Эта страница отображается, нажмите кнопку **входа** ссылку.
5. Нажмите кнопку **забыли пароль?** ссылку, чтобы отобразить **забытый пароль** страницы.
6. Введите адрес электронной почты и нажмите кнопку **отправить** кнопку, чтобы отправить сообщение электронной почты на ваш адрес, который позволит вам сбросить пароль.   
 Проверьте учетную запись почты и щелкните ссылку для отображения **Смена пароля** страницы.
7. На **Смена пароля** введите вашей электронной почты, пароль и подтвержденный пароль. Затем нажмите клавишу **Сброс** кнопки.  
 Если вы успешно сбросить свой пароль, **пароль изменен** будет отображена страница. Теперь можно войти, используя новый пароль.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Повторно отправить ссылку по электронной почте подтверждение

Когда пользователь создает новую локальную учетную запись, они являются уведомления по электронной почте, они должны использовать для входа ссылку для подтверждения. Если пользователь случайно удаляет сообщение электронной почты с подтверждением или сообщение никогда не будет доставлено, им потребуется повторная отправка ссылку для подтверждения. Следующие изменения кода показано, как включить эту возможность.

1. В Visual Studio откройте **Login.aspx.cs** кода и добавьте следующий обработчик событий после `LogIn` обработчик событий:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Изменить `LogIn` обработчик событий в *Login.aspx.cs* кода, изменив код, выделены желтым цветом следующим образом: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Обновление *Login.aspx* страницы путем добавления кода, выделены желтым цветом следующим образом: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Удалить все учетные записи в **AspNetUsers** таблица, которая содержит псевдоним электронной почты, необходимо протестировать.
5. Запустите приложение (**F5**) и зарегистрируйте свой адрес электронной почты.
6. Перед подтверждением вашей новой учетной записи по электронной почте, была недавно отправлена, попробуйте войти с использованием новой учетной записи.  
 Вы увидите, что не удается выполнить вход и необходимо наличие учетной записи электронной почты подтверждена. Кроме того теперь можно отправить сообщение с подтверждением электронную почту.
7. Введите адрес электронной почты и пароль, затем нажмите клавишу **повторно отправить подтверждение** кнопки.
8. После подтверждения ваш адрес электронной почты, на основе только что отправлен по электронной почте сообщения войдите в приложение.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Устранение неполадок приложения

Если вы не получили сообщение электронной почты, содержащее ссылку, чтобы проверить учетные данные:

- Проверьте папку нежелательной или Нежелательная почта.
- Войдите в учетную запись SendGrid и выберите [действия электронной почты ссылка](https://sendgrid.com/logs/index).
- Точно определить, используется имя пользователя учетной записи SendGrid как *Web.config* значение вместо того чтобы ваш адрес электронной почты учетной записи SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

- [Ссылки на ASP.NET Identity, рекомендуется использовать ресурсы](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Подтверждение учетной записи и пароль восстановления в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Серия учебников по веб-форм ASP.NET - добавить поставщика OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Развертывание приложения безопасного ASP.NET Web Forms членства, OAuth и базы данных SQL в службе приложений Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Учебник ряда веб-форм ASP.NET - включить SSL для проекта](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
