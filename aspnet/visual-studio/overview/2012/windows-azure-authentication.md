---
uid: visual-studio/overview/2012/windows-azure-authentication
title: "Windows Azure Authentication | Документы Microsoft"
author: Rick-Anderson
description: "Инструменты Microsoft ASP.NET для Windows Azure Active Directory позволяет легко включить проверку подлинности для веб-приложений, размещенных в Windows Azure веб-сайтов..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4deb3536699f1ef3025f8858ee71a76a1c2def18
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="windows-azure-authentication"></a>Windows Azure проверка подлинности
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Средства Microsoft ASP.NET для Windows Azure Active Directory позволяет легко включить проверку подлинности для веб-приложений, размещенных на [веб-сайтов Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Проверка подлинности Windows Azure можно использовать для проверки подлинности пользователей Office 365 в организации, корпоративных учетных записей, синхронизированные из локальной службы Active Directory или пользователей, созданных в собственный домен Windows Azure Active Directory. Включение проверки подлинности Windows Azure настраивает приложение для проверки подлинности пользователей с помощью одного [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) клиента.
> 
> Средство проверки подлинности ASP.NET Windows Azure не поддерживается для веб-ролей в облачной службе, но мы планируем в будущем выпуске. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) поддерживается в веб-роли Windows Azure.
> 
> Дополнительные сведения о том, как настроить синхронизацию между вашей локальной Active Directory и клиент Windows Azure Active Directory см. в разделе [использование AD FS 2.0 для реализации и управления единым входом](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory в настоящее время доступен в виде [освободить предварительной версии службы](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Требования:

- Visual Studio 2012 или [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Веб-средств расширения для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) или [расширения веб-средства для Visual Studio Express 2012 г.](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools для Windows Azure Active Directory — Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) или [Microsoft ASP.NET Tools для Windows Azure Active Directory — Visual Studio Express 2012 для Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Создание веб-приложения ASP.NET с помощью Visual Studio 2012

Все веб-приложения можно создать с помощью Visual Studio 2012, в этом учебнике используется шаблон интрасети ASP.NET MVC.

1. Создайте новое приложение интрасети MVC 4 ASP.NET и примите параметры по умолчанию. (Он должен быть In **проверка сс** net и не в **ажите** net проекта).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Включение проверки подлинности Azure окна (Если вы являетесь глобальным администратором принцип)

Если нет существующего клиента Windows Azure Active Directory (например, с помощью существующую учетную запись Office 365) можно создать новый клиент, зарегистрировавшись для [новую учетную запись Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. В меню Проект выберите **включить проверку подлинности Windows Azure**:  
  
 ![](windows-azure-authentication/_static/image2.png)

2. Введите имя домена для вашего клиента Windows Azure Active Directory (например, contoso.onmicrosoft.com) и нажмите кнопку **включить**:

![](windows-azure-authentication/_static/image3.png)

3. В веб-проверки подлинности диалоговое окно входа с правами администратора для вашего клиента Windows Azure Active Directory:  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Включите Windows Azure, не имеющий прав администратора из принцип

Если у вас прав глобального администратора для вашего клиента Windows Azure Active Directory, можно снять флажок для подготовки приложения.

![](windows-azure-authentication/_static/image6.png)

В диалоговом окне отобразится **домена**, **ИД участник приложения** и **URL-адрес ответа** , необходимые для подготовки приложения с Azure Active Directory Принцип. Необходимо предоставить эти сведения кому-либо обладающего привилегиями, достаточными для подготовки приложения. В разделе[как реализовать единый вход в Windows Azure Active Directory - приложение ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) подробные сведения о том, как вручную создать участника службы с помощью командлета.  
После успешной подготовки приложения можно щелкнуть **обновить файл web.config с помощью выбранных параметров по-прежнему**. Если вы хотите продолжать разработку приложения при ожидании инициализации нежелательно, можно щелкнуть **закрыть, чтобы хранить параметры в файле проекта**. В следующий раз, вызывать включить проверку подлинности Windows Azure и снимите флажок подготовки, вы увидите те же параметры, и можно щелкнуть **Продолжить**, нажмите кнопку **применить эти параметры в файле web.config**.

1. Подождите, пока приложение настроено для аутентификации Windows Azure и настроена для поддержки Windows Azure Active Directory.
2. После включения проверки подлинности Windows Azure для приложения щелкните **закрыть:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Нажмите клавишу F5 для запуска приложения. Вы автоматически получите перенаправляется на страницу входа. Использовать учетные данные пользователя принцип каталог для входа в приложение...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Так как приложение в настоящее время использует самозаверяющий тестовый сертификат вы получите предупреждение от браузера, сертификат не был выдан доверенным центром сертификации.

    Это предупреждение можно пропустить во время локальной разработки, щелкнув **Продолжить открытие этого веб-сайта:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Теперь успешного входа приложение, используя проверку подлинности Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Включение Windows Azure проверка подлинности вносит следующие изменения в приложение:

- Приложения для защиты сайта перекрестную подделки запросов ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) класса ( *приложения\_Start\AntiXsrfConfig.cs* ) добавляется в проект.
- Пакеты NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` добавляется в проект.
- Настроек Windows Identity Foundation в приложении будет настроен для приема токенов безопасности из клиента Windows Azure Active Directory. Щелкните ниже, чтобы увидеть изменения, внесенные в расширенное представление *Web.config* файла.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Будет использован субъекта-службы для приложения в клиенте Windows Azure Active Directory.
- Протокол HTTPS включен.

## <a name="deploy-the-application-to-windows-azure"></a>Развертывание приложения в Windows Azure

Полные инструкции см. в разделе [развертывание веб-приложения ASP.NET на веб-сайта Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Чтобы опубликовать приложение с использованием проверки подлинности Windows Azure для веб-сайта Azure:

1. Щелкните приложение правой кнопкой мыши и выберите **публикации:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Из диалогового окна веб-публикация Загрузите и импортируйте профиль публикации для веб-сайта Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Подключения** вкладке показана **URL-адрес назначения** (открытый разворота URL-адрес приложения). Нажмите кнопку **проверить подключение** можно проверить подключение:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Публикацию на этом веб-сайте Azure ранее имеет смысл вернуть **удалить дополнительные файлы в месте назначения** параметр, чтобы обеспечить приложение публикует без ошибок. Обратите внимание **включить проверку подлинности Windows Azure** slected — флажок.  

    ![](windows-azure-authentication/_static/image10.png)
5. Необязательно: На **предварительного просмотра** вкладке **начать просмотр** Чтобы просмотреть файлы, развернутые.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Нажмите кнопку **публикации.**

    Будет предложено включить проверку подлинности Windows Azure для конечного узла. Нажмите кнопку **включить** продолжить:

    ![](windows-azure-authentication/_static/image11.png)
7. Введите учетные данные администратора для вашего клиента Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. После успешной публикации приложения браузер откроет опубликованный веб-узел.

    > [!NOTE]
    > Он может занять до пяти минут (обычно необходимо меньше) для вашего приложения, полностью подготовки к работе с Windows Azure Active Directory после включения проверки подлинности Windows Azure для конечного узла. При первом запуске приложения при получении ошибки ACS50001: проверяющей стороной с именем [область] не найден, подождите несколько минут и попробуйте запустить приложение еще раз.
9. При появлении соответствующего запроса войдите в систему как пользователь в каталоге:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Вы успешно выполнен вход в Azure размещенного приложения, использующего проверку подлинности Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Известные проблемы

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>При использовании проверки подлинности Windows Azure < o: p >< сбой авторизации на основе ролей / o: p >

Проверка подлинности Windows Azure не предоставляет в настоящее время утверждение необходимые роли, что можно провести авторизации на основе ролей. Роль пользователя, прошедшего проверку подлинности должна осуществляться вручную из Windows Azure Active Directory. < o: p >< / o: p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Просмотр приложения с результатами проверки подлинности Windows Azure в сообщении об ошибке «домен пользователя, зарегистрированного в системе (live.com) не совпадает с любой ACS20016 разрешено домена этого STS» < o: p >< / o: p >

Если вы уже вошли в учетную запись Майкрософт (например, hotmail.com, live.com, outlook.com) и предпринимается попытка получить доступ к приложению, включил проверку подлинности Windows Azure может получить ответ ошибке 400, так как домен учетной записи Майкрософт не является службой Windows Azure Active Directory. Чтобы войти в приложение, выйдите из учетной записи Майкрософт сначала. < o: p >< / o: p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Вход в приложение с включена проверка подлинности Windows Azure и X509CertificateValidationMode, отличное от None в результате ошибки проверки сертификатов для сертификата accounts.accesscontrol.windows.net < o: p >< / o: p >

Проверка сертификата не является обязательным и следует отключить. Отпечаток сертификата издателя проверенного WSFederationAuthenticationModule. < o: p >< / o: p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>При попытке включить проверку подлинности Windows Azure веб-проверки подлинности диалоговое окно показывает ошибку «ACS20016: домен пользователя, зарегистрированного в системе (contoso.onmicrosoft.com) не совпадает с любой допустимому домену STS.» < o: p >< / o: p >

Появится это сообщение об ошибке при был ранее успешно выполнен вход с помощью Windows Azure Active Directory учетной записи из в одном процессе Visual Studio. Выйдите из указанной учетной записи или перезапустите Visual Studio. Если ранее в систему и выбран параметр «Оставаться в системе», то может потребоваться очистить файлы cookie браузер. < o: p >< / o: p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Запрос не является допустимым сообщением протокола WS-Federation < o: p >< / o: p >

Это может произойти, если вы уже вошли с использованием некоторых других Microsoft ID к одной из служб Azure. Окно обозревателя используются закрытые, например InPrivate в браузере IE или анонимный в браузере Chrome или очистить все файлы cookie. <o:p></o:p>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Microsoft ASP.NET Tools для Windows Azure Active Directory — Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) — Vittorio Bertocci
- [Windows Azure функции: удостоверение](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: разработка приложений для вашей организации](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: разработка приложений для нескольких организаций](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Как реализовать единый вход в Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Единый вход в Windows Azure Active Directory: глубокое погружение](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) — Vittorio Bertocci
- [Использование AD FS 2.0 для реализации и управления единым входом](https://technet.microsoft.com/library/jj205462.aspx)
