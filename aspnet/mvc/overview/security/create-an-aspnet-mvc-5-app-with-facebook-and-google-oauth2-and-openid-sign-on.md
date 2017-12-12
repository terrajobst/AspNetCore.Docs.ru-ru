---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: "Создать MVC 5 приложения с Facebook, Twitter, LinkedIn и Google OAuth2 единого входа (C#) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник показывает, как для создания веб-приложения ASP.NET MVC 5, позволяющий пользователям выполнять вход с использованием OAuth 2.0 с учетными данными из внешних предварительная пр..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: aaa061e61b9bab5b33083851624f0487b2cf6473
ms.sourcegitcommit: ccf08615ad59bc6f654560de33b93396113a2eb0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2017
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Создание приложения ASP.NET MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 единого входа (C#)
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Этого учебника показано, как построить веб-приложение ASP.NET MVC 5, который позволяет пользователям входить в систему с помощью [OAuth 2.0](http://oauth.net/2/) с учетными данными из внешнего поставщика аутентификации, например Facebook, Twitter, LinkedIn, Microsoft или Google. Для простоты учебнике рассматривается работа с учетными данными с Facebook и Google.
> 
> Включение этих учетных данных на веб-узлах обеспечивает значительное преимущество, так как миллионы пользователи уже имеют учетные записи, с помощью этих внешних поставщиков. Эти пользователи могут иметь более необходимо, чтобы зарегистрироваться для веб-узла, если они не нужно создавать и запоминать новый набор учетных данных.
> 
> См. также [приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Учебник также показано, как добавить данные профиля для пользователя и как использовать API членства для добавления ролей. Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (следуйте мне в Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Начало работы

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка Visual Studio [2013 обновление 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии. Для получения справки по Dropbox, GitHub, Linkedin, Instagram, буфера, salesforce, поток данных, Exchange стека, Tripit, twitch, Twitter, Yahoo и многое другое см. в этой [универсальное руководство по](http://www.oauthforaspnet.com/).

> [!NOTE]
> Необходимо установить Visual Studio [2013 обновление 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии с помощью Google OAuth 2 и отлаживать локально без предупреждения SSL.


Нажмите кнопку **новый проект** из **запустить** страницы, или можно использовать меню и выберите **файл**, а затем **новый проект**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Создание первого приложения

Нажмите кнопку **новый проект**, а затем выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**. Назовите проект «MvcAuth» и нажмите кнопку **ОК**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC**. Если проверка подлинности не **отдельных учетных записей пользователей**, нажмите кнопку **изменить аутентификацию** и выберите пункт **индивидуальные учетные записи**. Проверив **узел в облаке**, приложение будет очень легко разместить в Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Если вы выбрали **узел в облаке**, завершения Настройка диалогового окна.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Использовать NuGet для обновления до последней по промежуточного слоя OWIN

Используйте диспетчер пакетов NuGet для обновления [по промежуточного слоя OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Выберите **обновления** в левом меню. Можно щелкнуть **обновить все** кнопки, или можно выполнить поиск только пакеты OWIN (как показано на следующем рисунке):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

На приведенном ниже рисунке показаны только пакеты OWIN.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Из пакета Диспетчер консоли (PMC), можно ввести `Update-Package` команду, которая обновит все пакеты.

Нажмите клавишу **F5** или **Ctrl + F5** для запуска приложения. На рисунке ниже номер порта — 1234. При запуске приложения вы увидите другой номер порта.

В зависимости от размера окна браузера, может потребоваться щелкните значок навигации для просмотра **Главная**, **о**, **контакт**, **зарегистрировать**и **входа** ссылки.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Настройка SSL в проекте

Чтобы подключиться к поставщиков проверки подлинности, например Google и Facebook, необходимо настроить IIS Express для использования протокола SSL. Очень важно сохранить с помощью протокола SSL после входа в систему и не удалять обратно на HTTP, cookie-файл входа как секрет имя пользователя и пароль, а не с помощью протокола SSL при отправке в открытым текстом по каналу связи. Кроме того, вы уже сделали времени для выполнения установки соединения и защитить канал (являющийся массовой делает HTTPS медленнее, чем HTTP) перед запуском конвейера MVC, поэтому перенаправление обратно HTTP после вы выполнили вход в не текущего запроса или сделать будущее запросы гораздо быстрее.

1. В **обозревателе решений**, нажмите кнопку **MvcAuth** проекта.
2. Нажмите клавишу F4 для отображения свойств проекта. Кроме того, в **представление** меню можно выбрать **окно свойств**.
3. Изменение **включен протокол SSL** значение True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Скопируйте URL-адрес SSL (который может быть `https://localhost:44300/` Если вы создали проекты SSL).
5. В **обозревателе решений**, щелкните правой кнопкой мыши **MvcAuth** проект и выберите **свойства**.
6. Выберите **Web** вкладку, а затем вставьте URL-адрес SSL в **URL-адрес проекта** поле. Сохраните файл (Ctl + S). Вам потребуется этот URL-адрес для настройки проверки подлинности приложения Facebook и Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Добавить [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) атрибут `Home` контроллера требовать все запросы должны использовать HTTPS. Более безопасный подход — добавить [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) фильтра в приложение. См. в разделе &quot;защиты приложения с SSL и авторизовать атрибутом&quot; в моей tutoral [создать приложение ASP.NET MVC с помощью проверки подлинности и баз данных SQL Server и развернуть в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ниже приведен фрагмент контроллера Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Нажмите CTRL+F5, чтобы запустить приложение. Если вы уже установили сертификат в прошлом, можно пропустить оставшейся части этого раздела и перехода к [Создание приложения Google OAuth 2 и подключение приложения к проекту](#goog), в противном случае следуйте инструкциям, чтобы доверять собственной подписью сертификат, созданный IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Чтение **предупреждение системы безопасности** диалоговое окно и нажмите кнопку **Да** Если вы хотите установить сертификат, представляющий localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Показывает, IE *Главная* страницы и отсутствуют предупреждения SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome также принимает сертификат и будет показано содержимое HTTPS без предупреждения. Firefox использует собственное хранилище сертификатов, поэтому он будет отображаться предупреждение. Для нашего приложения могут безопасно щелкните **я понимаю последствия**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Создание приложения Google OAuth 2 и подключение приложения в проект

1. Перейдите к [Google Developers Console](https://console.developers.google.com/).
1. Если проект перед еще не создан, выберите **учетные данные** левой вкладке, а затем выберите **создать**.
1. На левой вкладке нажмите кнопку **учетные данные**.
1. Нажмите кнопку **создать учетные данные** затем **идентификатор клиента OAuth**. 

    1. В **создать идентификатор клиента** диалоговое окно, оставьте значение по умолчанию **веб-приложение** для типа приложения.
    2. Задать **право JavaScript** источники, которые можно использовать выше URL-адрес SSL (`https://localhost:44300/` Если вы создали проекты SSL)
    3. Задать **авторизованные URI перенаправления** для:  
         `https://localhost:44300/signin-google`
5. Пункт меню экран согласия OAuth, а затем задайте адрес и продукта имени электронной почты. После завершения работы щелкните форму **Сохранить**.
6. Пункт меню библиотеки, поиска **API Google +**, щелкните его, а затем нажмите клавишу Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 На рисунке ниже показана включен API-интерфейсы.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Посетите из Google API-интерфейсы API диспетчера, **учетные данные** вкладку, чтобы получить **идентификатор клиента**. Загрузите, сохранить файл JSON с секретными данными приложения. Скопируйте и вставьте **ClientId** и **ClientSecret** в `UseGoogleAuthentication` найти метод в *Startup.Auth.cs* файла в *App_Start* папки. **ClientId** и **ClientSecret** значений, приведенных ниже примеров и не работают.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. Учетная запись и учетные данные добавляются выше, чтобы не усложнять в образце кода. В разделе [советы и рекомендации по развертыванию пароли и другие конфиденциальные данные, ASP.NET и службы приложений Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Нажмите клавишу **CTRL + F5** для построения и запуска приложения. Нажмите кнопку **входа** ссылку.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. В разделе **используется другой службой для входа в**, нажмите кнопку **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Если пропустить любой из вышеперечисленных действий вы получите сообщение об ошибке HTTP 401. Повторная проверка выше действия. Если вы пропустите обязательный параметр (например **название продукта**), Добавление отсутствующих элементов и сохранить, может занять несколько минут для проверки подлинности для работы.
10. Вы будете перенаправлены на сайт google, можно будет ввести учетные данные.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. После ввода учетных данных, будет предложено предоставить разрешения для только что созданной веб-приложения:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Нажмите кнопку **принимать**. Вам будут перенаправляться к **зарегистрировать** страница MvcAuth приложения, где можно зарегистрировать учетную запись Google. Вы можете изменить имя локальной электронной почты регистрации, используемое для свою учетную запись Gmail, но обычно требуется сохранить псевдоним электронной почты по умолчанию (один используется для проверки подлинности). Щелкните ссылку **Регистрация**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Создать приложение в Facebook и подключить приложения в проект

Для проверки подлинности Facebook OAuth2 необходимо скопировать в проект некоторые параметры из приложения, создаваемые в Facebook.

1. В браузере перейдите к [https://developers.facebook.com/apps](https://developers.facebook.com/apps) и войдите в систему, указав учетные данные Facebook.
2. Если вы уже не зарегистрирован как разработчика Facebook, нажмите кнопку **зарегистрировать как разработчик** и следуйте инструкциям для регистрации.
3. На **приложения** щелкните **создать новое приложение**.

    ![Создайте новое приложение](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Введите **имя приложения** и **категории**, нажмите кнопку **Создание приложения**.

    Это должно быть уникальным для Facebook. **Пространство имен приложения** является частью URL-адрес, приложение будет использовать для доступа к приложению Facebook для проверки подлинности (например, https://apps.facebook.com/ {пространство имен приложения}). Если не указать **пространство имен приложения**, **идентификатор приложения** будет использоваться URL-адреса. **Идентификатор приложения** долго системой номер, который вы увидите на следующем шаге.

    ![Создание диалогового окна нового приложения](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Отправьте проверки безопасности standard.

    ![Проверка безопасности](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Выберите **параметры** для левого меню.![ Строка меню разработчика Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. На **основные** страницы «Параметры» выберите **добавить платформу** для указания, что добавляется приложение веб-сайта. ![Основные параметры](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Выберите **веб-сайт** из вариантов платформы.  
  
    ![Выбор платформы](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Обратите внимание на **идентификатор приложения** и **секрет приложения** , чтобы оба сертификата в MVC-приложения можно добавить позднее в этом учебнике. Кроме того, добавьте URL-адрес сайта (`https://localhost:44300/`) для тестирования приложения MVC. Кроме того, добавьте **адрес электронной почты**. Выберите **сохранить изменения**.   

    ![Страница сведений о основного приложения](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Обратите внимание, что можно будет только возможность проверки подлинности с помощью зарегистрированного псевдоним электронной почты. Другие пользователи и тестовые учетные записи не будет возможности регистрации. Вы можете предоставить доступ учетным записям других Facebook для приложения на Facebook **роль разработчика** вкладки.
10. В Visual Studio откройте *приложения\_Start\Startup.Auth.cs*.
11. Скопируйте и вставьте **AppId** и **секрет приложения** в `UseFacebookAuthentication` метод. **AppId** и **секрет приложения** значений, приведенных ниже примеров и не будет работать.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Нажмите кнопку **сохранить изменения**.
13. Нажмите клавишу **CTRL + F5** для запуска приложения.


Выберите **входа** для отображения страницы входа. Нажмите кнопку **Facebook** под **используйте для входа другую службу.**

Введите учетные данные Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Будет предложено предоставить разрешение на доступ к открытый профиль и список friend приложения.

![Сведения о приложении Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Вы вошли. Теперь можно зарегистрировать эту учетную запись с приложением.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

При регистрации, добавляется запись *пользователей* таблицы базы данных членства.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Проверьте данные членства

В **представление** меню, нажмите кнопку **обозревателя серверов**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Разверните **DefaultConnection (MvcAuth)**, разверните **таблиц**, щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers таблицы данных](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Добавление данных профиля в класс пользователя

В этом разделе вы добавите дату рождения и родного города пользовательских данных во время регистрации, как показано на следующем рисунке.

![раздел реестра с родного города и д.рожд.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Откройте *Models\IdentityModels.cs* и добавьте свойства Город Домашняя страница и Дата рождения файла:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Откройте *Models\AccountViewModels.cs* файла и набор свойств Город даты и дома в их создания `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Откройте *Controllers\AccountController.cs* и добавьте код для города Домашняя страница и даты рождения в `ExternalLoginConfirmation` метода действия, как показано:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Добавить дату рождения и родного города для *Views\Account\ExternalLoginConfirmation.cshtml* файла:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Удалите базу данных членства, чтобы можно было снова зарегистрировать вашу учетную запись Facebook с приложением и убедитесь, что можно добавить новую дату рождения и сведения о профиле родного города.

Из **обозревателе решений**, нажмите кнопку **Показать все файлы** значок, а затем щелкните правой кнопкой мыши *добавить\_Data\aspnet-MvcAuth -&lt;метки даты&gt;.mdf* и нажмите кнопку **удалить**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Из **средства** меню, нажмите кнопку **диспетчера пакетов NuGet**, нажмите кнопку **консоль диспетчера пакетов** (PMC). Введите следующие команды в PMC.

1. Enable-Migrations
2. Init-Migration
3. Update-Database

Запустите приложение и использовать для входа и регистрации некоторых пользователей FaceBook и Google.

## <a name="examine-the-membership-data"></a>Проверьте данные членства

В **представление** меню, нажмите кнопку **обозревателя серверов**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` И `BirthDate` поля будут показаны ниже.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Входа в систему с другой учетной записи и выход из приложения

Если вход в приложение с Facebook и затем выйдите из системы и попробуйте войти снова под другой учетной записью Facebook (используя тот же браузер), вам будет немедленно вход в систему предыдущей учетной записи Facebook, который использовался. Чтобы использовать другую учетную запись, необходимо перейти к Facebook и выйдите из системы на Facebook. Это же правило применяется к любой другой стороннего проверки подлинности поставщика. Кроме того можно войти под другой учетной записью, используя другой браузер.

## <a name="next-steps"></a>Дальнейшие действия

В разделе [введение Yahoo и LinkedIn OAuth поставщиков безопасности для OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) по Jerrie Pelser Yahoo и LinkedIn инструкции. См. в Jerrie довольно кнопки входа социальных сетей для ASP.NET MVC 5 для получения enable кнопки входа социальных сетей.

Выполните my учебника [создать приложение ASP.NET MVC с помощью проверки подлинности и баз данных SQL Server и развернуть в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), который продолжает этого учебника и отображает следующую:

1. Как развернуть приложение в Azure.
2. Как защитить приложение с ролями.
3. Как защитить приложение с [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) и [авторизовать](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) фильтры.
4. Инструкции по использованию API членства для добавления пользователей и ролей.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Можно даже запрашивает и голосовать о новых функциях, добавляемых к ASP.NET. Например, смогут проголосовать за это средство, [Создание и управление пользователями и ролями.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Хорошее описание принципов работы внешние службы проверки подлинности ASP.NET см. в разделе Robert McMurray [внешние службы проверки подлинности](https://asp.net/web-api/overview/security/external-authentication-services). Статья Ивана также переходит в подробно Включение проверки подлинности Майкрософт и Twitter. Отлично Tom Dykstra [EF и MVC учебника](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) показано, как работать с платформой Entity Framework.
