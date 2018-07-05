---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure проверка подлинности | Документация Майкрософт
author: Rick-Anderson
description: Средства Microsoft ASP.NET для Windows Azure Active Directory вы можете легко включить проверку подлинности для веб-приложений, размещенных в Windows Azure Web Sites...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: d46a673b75eba0e058c7e20b12f44caf4e2f4f50
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379797"
---
<a name="windows-azure-authentication"></a>Windows Azure проверка подлинности
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Microsoft ASP.NET средства для Windows Azure Active Directory вы можете легко включить проверку подлинности для веб-приложений, размещенных на [веб-сайтов Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Проверка подлинности Windows Azure можно использовать для проверки подлинности пользователей Office 365 из вашей организации, корпоративных учетных записей, синхронизированные из локальной службы Active Directory или пользователей, созданных в своем домене Windows Azure Active Directory. Включение проверки подлинности Windows Azure настраивает приложение для проверки подлинности пользователей с помощью одного [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) клиента.
> 
> Средство проверки подлинности ASP.NET Windows Azure не поддерживается для веб-ролей в облачной службе, но мы планируем сделать это в будущем выпуске. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) поддерживается в веб-ролях Windows Azure.
> 
> Дополнительные сведения по настройке синхронизации между вашей локальной Active Directory и клиенте Windows Azure Active Directory см. [использование AD FS 2.0 для реализации и управления единым входом](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory в настоящее время доступна [бесплатная Предварительная версия службы](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Требования:

- Visual Studio 2012 или [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Расширения для Visual Studio 2012 веб-инструментов](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) или [Web Tools Extensions для Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Средства Microsoft ASP.NET для Windows Azure Active Directory — Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) или [средств Microsoft ASP.NET для Windows Azure Active Directory — Visual Studio Express 2012 для Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Создание веб-приложения ASP.NET с помощью Visual Studio 2012

Можно создать любое веб-приложение с помощью Visual Studio 2012, в этом руководстве используется шаблон интрасети ASP.NET MVC.

1. Создайте новое приложение интрасети MVC 4 ASP.NET и примите значения по умолчанию. (Оно должно быть In **tra** net и не в **ажите** net проекта).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Включение проверки подлинности Azure окна (Если вы являетесь глобальным администратором принцип)

Если у вас нет существующего клиента Windows Azure Active Directory (например, через существующую учетную запись Office 365) можно создать новый клиент, зарегистрировавшись для [новую учетную запись Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. В меню "проект" выберите **включить проверку подлинности Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Введите имя домена для вашего клиента Windows Azure Active Directory (например, contoso.onmicrosoft.com) и нажмите кнопку **включить**:

![](windows-azure-authentication/_static/image3.png)

3. В веб-проверка подлинности диалоговое окно входа с правами администратора для вашего клиента Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Включите Windows Azure, без прав администратора из принцип

Если у вас нет права глобального администратора для вашего клиента Windows Azure Active Directory, можно снять флажок для подготовки приложения.

![](windows-azure-authentication/_static/image6.png)

В диалоговом окне отобразится **домена**, **идентификатор субъекта приложения** и **URL-адрес ответа** , необходимые для подготовки приложения с Azure Active Directory Принцип. Необходимо предоставить эти сведения тому, кто имеет недостаточно привилегий для подготовки приложения. См. в разделе[как реализовать единый вход Windows Azure Active Directory - приложение ASP.NET с](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) Дополнительные сведения о том, как использовать командлет для создания субъекта-службы вручную.  
После успешной подготовки приложения вы можете щелкнуть **постоянно обновлять web.config с помощью выбранных параметров**. Если вы хотите продолжить разработку приложения во время ожидания подготовки нежелательно, можно щелкнуть **близко к хранить параметры в файле проекта**. В следующий раз, вызвать включить проверку подлинности Windows Azure и снимите этот флажок, подготовки, вы увидите те же параметры, и можно щелкнуть **Продолжить**, затем нажмите кнопку, **применить эти параметры в файле web.config**.

1. Подождите, пока приложение настраивается для проверки подлинности Windows Azure и подготовлены с помощью Windows Azure Active Directory.
2. После включения проверки подлинности Windows Azure для вашего приложения, нажмите кнопку **закрыть:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Нажмите клавишу F5 для запуска приложения. Вы автоматически получите перенаправлены на страницу входа. Использовать учетные данные пользователя принцип каталог для входа в приложение...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Так как используется самозаверяющий тестовый сертификат для вашего приложения вы получите предупреждение от браузера, сертификат не был выдан доверенным центром сертификации.

    Это предупреждение можно спокойно проигнорировать во время локальной разработки, щелкнув **Продолжить открытие этого веб-сайта:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Теперь успешного входа приложение с помощью Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Включение Windows Azure проверка подлинности вносит следующие изменения в приложение:

- Подделка защита Anti-CROSS-Site ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) класса ( *приложения\_Start\AntiXsrfConfig.cs* ) добавляется в проект.
- Пакеты NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` добавляется в проект.
- Параметры Windows Identity Foundation в приложение будут настроены на прием токенов безопасности из клиента Windows Azure Active Directory. Щелкните ниже, чтобы увидеть изменения, внесенные в развернутое представление *Web.config* файл.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Подготавливается субъекта-службы для приложения в клиенте Windows Azure Active Directory.
- Протокол HTTPS включен.

## <a name="deploy-the-application-to-windows-azure"></a>Развертывание приложения в Windows Azure

Полные инструкции см. в разделе [развертывание веб-приложения ASP.NET для веб-сайта Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Чтобы опубликовать приложение с помощью Windows Azure на веб-сайте Azure:

1. Щелкнуть правой кнопкой мыши приложение и выберите **публикации:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. В диалоговом окне публикации веб-сайта скачайте и импортируйте профиль публикации для веб-сайт Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Подключения** вкладке показано **URL-адрес назначения** (общедоступный с выходом URL-адрес для вашего приложения). Нажмите кнопку **проверить подключение** Чтобы проверить подключение:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Если вы опубликовали этот веб-сайт Azure ранее рекомендуется проверить **удалять дополнительные файлы в месте назначения** параметр, чтобы обеспечить приложение публикует аккуратно. Обратите внимание, что **включить проверку подлинности Windows Azure** флажке slected.  

    ![](windows-azure-authentication/_static/image10.png)
5. Необязательно: На **предварительной версии** вкладке **начать просмотр** для просмотра развернутых файлов.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Нажмите кнопку **публикации.**

    Вам будет предложено включить проверку подлинности Windows Azure для целевого узла. Нажмите кнопку **включить** для продолжения:

    ![](windows-azure-authentication/_static/image11.png)
7. Введите учетные данные администратора для вашего клиента Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. После успешной публикации приложения браузер откроет для опубликованного веб-сайта.

    > [!NOTE]
    > Может занять до пяти минут (обычно необходимо меньше) для вашего приложения, чтобы полностью подготовлены с помощью Windows Azure Active Directory после включения проверки подлинности Windows Azure для целевого узла. При первом запуске приложения при получении ошибки ACS50001: не найдена проверяющая сторона с именем «[realm]», подождите несколько минут и попробуйте запустить приложение еще раз.
9. При появлении запроса, войдите в качестве пользователя в каталоге:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Вы успешно теперь выполнили вход в Azure в которых размещено приложение, с помощью Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Известные проблемы

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Ролевая Авторизация происходит сбой при проверке подлинности Windows Azure < o:p >< / o:p >

Проверка подлинности Windows Azure не поддерживает в настоящее время утверждение необходимые роли таким образом, можно выполнять авторизацию на основе ролей. Роль пользователя, прошедшего проверку подлинности должна осуществляться вручную из Windows Azure Active Directory. < o:p >< / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Просмотр приложению, с помощью проверки подлинности Windows Azure приводит к ошибке «ACS20016 домен вошедшего в систему пользователя (live.com) не совпадает ни разрешено домена этого STS» < o:p >< / o:p >

Если вы уже вошли с учетной записью Майкрософт (например, hotmail.com, live.com, outlook.com), и вы попытаетесь получить доступ к приложению, для которой проверка подлинности Windows Azure может получить ответ ошибке 400, так как домен учетной записи Майкрософт не является службой Windows Azure Active Directory. Чтобы войти в приложение, выйти из учетной записи Майкрософт сначала. < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Вход в приложение с включенной проверкой подлинности Windows Azure и X509CertificateValidationMode, отличное от None приводит ошибки проверки сертификатов для сертификата accounts.accesscontrol.windows.net < o:p >< / o:p >

Проверка сертификата не является обязательным и следует отключить. Отпечаток издателя сертификата проверяется с WSFederationAuthenticationModule. < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>При попытке включить проверку подлинности Windows Azure в диалоговом окне веб-проверки подлинности отображаются ошибки «ACS20016: домен пользователь (contoso.onmicrosoft.com) не соответствует любой допустимому домену STS.» < o:p >< / o:p >

Эта ошибка наблюдается при был ранее успешно выполнен вход через Windows Azure Active Directory учетную запись из в одном процессе Visual Studio. Выйдите из указанной учетной записи или перезапустите Visual Studio. Если вы ранее вошел в систему и выбрали вариант «Оставаться в системе», то может потребоваться очистить файлы cookie браузер. < o:p >< / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Запрос не является допустимым сообщением протокола WS-Federation < o:p >< / o:p >

Это может произойти, если вы уже вошли в систему с помощью некоторых других идентификатора Майкрософт для одной из служб Azure. Окно браузера для частного использования, например InPrivate в Internet Explorer или режиме InPrivate в браузере Chrome или очистить все файлы cookie. < o:p >< / o:p >

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Средства Microsoft ASP.NET для Windows Azure Active Directory — Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) — Витторио Берточчи
- [Windows Azure функции: удостоверение](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: разработка приложений для вашей организации](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: разработка приложений для нескольких организаций](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Как реализовать единый вход в Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Единый вход в Windows Azure Active Directory: глубокое погружение в обработку](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) — Витторио Берточчи
- [Использование AD FS 2.0 для реализации и управления единым входом](https://technet.microsoft.com/library/jj205462.aspx)
