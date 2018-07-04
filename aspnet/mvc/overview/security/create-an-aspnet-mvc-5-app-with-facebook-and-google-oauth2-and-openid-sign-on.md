---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Создание MVC 5 приложения с помощью Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этот учебник показывает, как создавать веб-приложения ASP.NET MVC 5, который позволяет пользователям выполнять вход с помощью OAuth 2.0 с учетными данными из внешних проверка...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388770"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Создать приложение ASP.NET MVC 5 с помощью Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этом руководстве показано, как построить веб-приложение ASP.NET MVC 5, которая позволяет пользователям входить в систему с помощью [OAuth 2.0](http://oauth.net/2/) с учетными данными от поставщика внешней проверки подлинности, таких как Facebook, Twitter, LinkedIn, Microsoft или Google. Для простоты это руководство посвящено работе с учетными данными с Facebook и Google.
> 
> Включение эти учетные данные в веб-сайтов обеспечивает значительное преимущество, так как миллионам пользователей уже есть учетные записи с помощью этих внешних поставщиков. Эти пользователи могут быть более необходимо, чтобы зарегистрироваться для веб-узла, если они не нужно создавать и запоминать новый набор учетных данных.
> 
> См. также [приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> В руководстве также показано, как добавить данные профиля пользователя, а также как использовать API членства для добавления ролей. Это руководство было написано с [Рик Андерсон](https://blogs.msdn.com/rickAndy) (выполните мне в Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Начало работы

Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка Visual Studio [2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии. Получить справку по Dropbox, GitHub, Linkedin, Instagram, буфер, Salesforce, поток данных, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! и многое другое, см. в этом [пример проекта](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Необходимо установить Visual Studio [2013 с обновлением 3](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии с помощью Google OAuth 2 и отлаживать локально без предупреждений SSL.


Нажмите кнопку **новый проект** из **запустить** страницы, или можно использовать меню и выберите **файл**, а затем **новый проект**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Создание первого приложения

Нажмите кнопку **новый проект**, а затем выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**. Имя проекта «MvcAuth» и нажмите кнопку **ОК**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC**. Если проверка подлинности не **учетные записи отдельных пользователей**, нажмите кнопку **изменить способ проверки подлинности** и выберите **учетные записи отдельных пользователей**. Проверив **разместить в облаке**, приложение будет очень легко разместить в Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Если вы выбрали **разместить в облаке**, заполните поля диалогового окна настройки.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Используйте NuGet для обновления до последней по промежуточного слоя OWIN

Используйте диспетчер пакетов NuGet для обновления [по промежуточного слоя OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Выберите **обновления** в меню слева. Вы можете щелкнуть **обновить все** кнопку можно искать только пакеты OWIN (показано на рисунке ниже):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

В приведенном ниже рисунке показаны только пакеты OWIN.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Из консоли диспетчера пакетов (PMC), можно ввести `Update-Package` команду, которая будет обновить все пакеты.

Нажмите клавишу **F5** или **Ctrl + F5** для запуска приложения. На рисунке ниже номер порта — 1234. При запуске приложения вы увидите другой номер порта.

В зависимости от размера окна браузера может потребоваться щелкнуть значок навигации, чтобы см. в разделе **Главная**, **о**, **контакт**, **зарегистрировать**и **вход** ссылки.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Настройка SSL в проекте

Чтобы подключиться к поставщикам проверки подлинности, таких как Google и Facebook, необходимо настроить IIS Express для использования протокола SSL. Необходимо продолжать использовать SSL после входа в систему и не удалять обратно к HTTP, ваш файл cookie входа — так же, как секрет, как имя пользователя и пароль и без использования SSL, вы отправляете его в открытый текст по каналу связи. Кроме того, вы уже сделали время для выполнения установки соединения и обеспечьте безопасность канала (это основная часть делает HTTPS медленнее, чем HTTP) перед запуском конвейера MVC, поэтому перенаправление обратно HTTP после входа не текущего запроса или сделать будущее запросы гораздо быстрее.

1. В **обозревателе решений**, нажмите кнопку **MvcAuth** проекта.
2. Нажмите клавишу F4, чтобы показать свойства проекта. Кроме того, из **представление** меню можно выбрать **окно "Свойства"**.
3. Изменение **SSL включен** значение true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Скопируйте URL-адрес SSL (который может быть `https://localhost:44300/` Если вы создали другие проекты SSL).
5. В **обозревателе решений**, щелкните правой кнопкой мыши **MvcAuth** проекта и выберите **свойства**.
6. Выберите **Web** вкладке, а затем вставьте URL-адрес SSL в **URL-адрес проекта** поле. Сохраните файл (Ctl + S). Вам потребуется этот URL-адрес для настройки проверки подлинности приложения Facebook и Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Добавить [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) атрибут `Home` контроллера для всех запросов необходимо использовать протокол HTTPS. Более безопасный подход заключается в добавлении [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) фильтр к приложению. См. в разделе &quot;защиты приложения с помощью SSL и атрибута авторизации&quot; в моей tutoral [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ниже приведен фрагмент контроллера Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Нажмите CTRL+F5, чтобы запустить приложение. Если вы установили сертификат в прошлом, вы можете пропустить оставшейся части этого раздела и перейти к [Создание приложения Google для oauth2 и подключение приложения к проекту](#goog), в противном случае следуйте инструкциям, чтобы доверия к самозаверяющему сертификат, созданный IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Чтение **предупреждение системы безопасности** диалоговое окно, а затем нажмите кнопку **Да** Если вы хотите установить сертификат, представляющий localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE откроется *Главная* страницы и без предупреждений SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome также принимает сертификат и будет показано содержимое HTTPS без предупреждения. Firefox использует собственное хранилище сертификатов, поэтому он будет отображаться предупреждение. Для нашего приложения могут безопасно щелкните **я понимаю последствия**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Создание приложения Google для oauth2 и подключение приложения к проекту

> [!WARNING]
> Актуальные инструкции Google OAuth, см. в разделе [Google Настройка проверки подлинности в ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Перейдите к [Google Developers Console](https://console.developers.google.com/).
2. Если вы еще не создали проект перед, выберите **учетные данные** на левой вкладке, а затем выберите **создать**.
3. На левой вкладке щелкните **учетные данные**.
4. Нажмите кнопку **создайте учетные данные** затем **идентификатор клиента OAuth**. 

    1. В **Создание идентификатора клиента** диалоговое окно, оставьте значение по умолчанию **веб-приложение** для типа приложения.
    2. Задайте **авторизованных JavaScript** источников на приведенном выше URL-адрес SSL (`https://localhost:44300/` Если вы создали другие проекты SSL)
    3. Задайте **авторизованные URI перенаправления** для:  
         `https://localhost:44300/signin-google`
5. Щелкните пункт меню экран согласия OAuth, а затем задайте адрес и продукта имени электронной почты. После завершения щелкните форму **Сохранить**.
6. Пункт меню библиотеки, поиска **Google + API**, щелкните его, а затем нажмите клавишу Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   На рисунке ниже показана включенных API-интерфейсов.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Посетите из API-интерфейсов API диспетчера Google, **учетные данные** tab, чтобы получить **идентификатор клиента**. Загрузки, чтобы сохранить файл JSON с секретными данными приложения. Скопируйте и вставьте **ClientId** и **ClientSecret** в `UseGoogleAuthentication` найти метод в *Startup.Auth.cs* файл *App_Start* папки. **ClientId** и **ClientSecret** значений, приведенных ниже примеров и не работают.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде. Запись и учетные данные добавляются в код выше для простоты в примере. См. в разделе [советы и рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и службы приложений Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Нажмите клавишу **CTRL + F5** для сборки и запуска приложения. Нажмите кнопку **вход** ссылку.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. В разделе **вход с помощью другой службы**, нажмите кнопку **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Если вы упустите какие-либо шаги выше вы получите ошибку HTTP 401. Проверьте выполненные действия выше. Если вы пропустите обязательный параметр (например **название продукта**), чтобы добавить отсутствующий элемент и сохраните; может занять несколько минут для проверки подлинности для работы.
10. Вы будете перенаправлены на сайт Google, где можно будет ввести свои учетные данные.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. После того как вы введете свои учетные данные, вам будет предложено предоставить разрешения на доступ к веб-приложение, которое вы только что создали:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Нажмите кнопку **принимать**. Вы будете перенаправлены обратно **зарегистрировать** страница MvcAuth приложения, где можно зарегистрировать учетную запись Google. Вы можете изменить имя регистрации локальной электронной почты для учетной записи Gmail, но обычно требуется сохранить псевдоним электронной почты по умолчанию (то есть той, которая используется для проверки подлинности). Щелкните ссылку **Регистрация**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Создать приложение в Facebook и подключить приложение к проекту

> [!WARNING]
> Актуальные инструкции для проверки подлинности Facebook OAuth2, см. в разделе [проверки подлинности Facebook, Настройка](/aspnet/core/security/authentication/social/facebook-logins)

Для проверки подлинности Facebook OAuth2 вам нужно скопировать в свой проект некоторые параметры из приложения, создаваемые в Facebook.

1. В окне браузера перейдите к [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) и войдите в систему, введя свои учетные данные Facebook.
2. Если вы еще не зарегистрировался в качестве разработчика Facebook, щелкните **регистрация в качестве разработчика** и следуйте инструкциям для регистрации.
3. На **приложений** щелкните **создать новое приложение**.

    ![Создание нового приложения](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Введите **имя_приложения** и **категории**, затем нажмите кнопку **создать приложение**.

    <strong>Пространство имен приложения</strong> является частью URL-адрес, приложение будет использовать для доступа к приложению Facebook для проверки подлинности (например, https\://apps.facebook.com/{App пространство имен}). Если вы не укажете <strong>пространство имен приложения</strong>, <strong>идентификатор приложения</strong> будет использоваться в качестве URL-адреса. <strong>Идентификатор приложения</strong> долго системой номер, который вы увидите на следующем шаге.

    ![Создать новое приложение](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Отправьте проверку безопасности standard.

    ![Проверка безопасности](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Выберите **параметры** для панели меню слева![ Строка меню разработчика Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. На **основные** страницы «Параметры» выберите **Добавление платформы** для указания, что при добавлении приложения веб-сайта. ![Основные параметры](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Выберите **веб-сайт** из вариантов платформы.  
  
    ![Выбор платформы](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Запомните или запишите вашей **идентификатор приложения** и **секрет приложения** таким образом, чтобы можно было добавить оба сертификата в приложении MVC далее в этом руководстве. Кроме того, добавьте URL-адрес сайта (`https://localhost:44300/`) для тестирования приложения MVC. Кроме того, добавьте **контактный адрес электронной почты**. Выберите **сохранить изменения**.   

    ![Странице сведений о простое приложение](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Обратите внимание, что вы только сможете пройти проверку подлинности с помощью зарегистрированного псевдоним электронной почты. Не будет возможности регистрации других пользователей и тестовых учетных записей. Вы можете предоставить доступ учетным записям других Facebook для приложения на Facebook **роль разработчика** вкладки.
10. В Visual Studio откройте *приложения\_Start\Startup.Auth.cs*.
11. Скопируйте и вставьте **AppId** и **секрет приложения** в `UseFacebookAuthentication` метод. **AppId** и **секрет приложения** значений, приведенных ниже примеров и не будет работать.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Нажмите кнопку **сохранить изменения**.
13. Нажмите клавишу **CTRL + F5** для запуска приложения.


Выберите **вход** на страницу входа. Нажмите кнопку **Facebook** под **вход с помощью другой службы.**

Введите свои учетные данные Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Вам будет предложено предоставить разрешение для приложения доступ к открытый профиль и списков друзей.

![Сведения о приложении Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Теперь вы вошли. Теперь вы можете зарегистрировать эту учетную запись с приложением.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

При регистрации, добавляется запись *пользователей* таблицы в базе данных членства.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Изучите данные членства

В **представление** меню, щелкните **обозревателя серверов**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Разверните **DefaultConnection (MvcAuth)**, разверните **таблиц**, щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![данные таблицы aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Добавление данных профиля в класс пользователя

В этом разделе вы добавите Дата рождения и родного города в пользовательских данных во время регистрации, как показано на следующем рисунке.

![REG родного города и д.рожд.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Откройте *Models\IdentityModels.cs* файл и добавьте свойства Город Домашняя страница и даты рождения:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Откройте *Models\AccountViewModels.cs* файл и задайте рождения даты и домашней свойства города в `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Откройте *файл Controllers\AccountController.cs* файл и добавьте код города Домашняя страница и даты рождения в `ExternalLoginConfirmation` метода действия, как показано:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Добавление даты рождения и родного города для *Views\Account\ExternalLoginConfirmation.cshtml* файла:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Удаление базы данных членства, чтобы можно было снова зарегистрировать учетную запись Facebook в приложения и убедитесь, что можно добавить новую дату рождения и сведения о профиле родного города.

Из **обозревателе решений**, нажмите кнопку **Показать все файлы** значок, а затем щелкните правой кнопкой мыши *добавить\_Data\aspnet-MvcAuth -&lt;метки даты&gt;.mdf* и нажмите кнопку **удалить**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Из **средства** меню, щелкните **диспетчера пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов** (PMC). Введите следующие команды в PMC.

1. Enable-Migrations
2. Add-Migration Init
3. Обновления базы данных

Запустите приложение и используйте FaceBook и Google для входа и регистрации некоторых пользователей.

## <a name="examine-the-membership-data"></a>Изучите данные членства

В **представление** меню, щелкните **обозревателя серверов**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Щелкните правой кнопкой мыши **AspNetUsers** и нажмите кнопку **Показать таблицу данных**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` И `BirthDate` поля, показаны ниже.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Входа в систему с другой учетной записи и выход из приложения

Если вход в приложение с помощью Facebook и затем выйдите и попробуйте войти снова использовать другую учетную запись Facebook (используя тот же самый браузер), вам будет немедленно войти в предыдущей учетной записи Facebook, которая использовалась. Чтобы использовать другую учетную запись, необходимо перейти к Facebook и выхода в Facebook. То же правило применяется к любой другой сторонний поставщик проверки подлинности субъекта. Кроме того можно войти с использованием другой учетной записи, используя другой браузер.

## <a name="next-steps"></a>Следующие шаги

См. в разделе [введение поставщиков безопасности Yahoo и LinkedIn OAuth для OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) по Pelser Джерри инструкции Yahoo и LinkedIn. См. в разделе Джерри довольно кнопки входа из социальных сетей для ASP.NET MVC 5, чтобы получить Включение кнопки входа из социальных сетей.

Следуйте указаниям руководства, Мой [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), который продолжает этом руководстве и отображаются следующие:

1. Как развернуть приложение в Azure.
2. Как защитить, это приложение с помощью ролей.
3. Как защитить приложения с помощью [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) и [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) фильтры.
4. Как использовать API членства для добавления пользователей и ролей.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить. Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Можно запросить и проголосовать за новые функции, добавляемых к ASP.NET. Например, вы можете проголосовать за это средство для [создания и управления пользователями и ролями.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Хорошее объяснение принципов работы внешние службы проверки подлинности ASP.NET, см. в разделе Robert McMurray [внешние службы проверки подлинности](https://asp.net/web-api/overview/security/external-authentication-services). Статью Роберта также подробно реализации функции аутентификации Microsoft и Twitter. Том Дайкстра отличную [руководство по EF и MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) показано, как работать с платформой Entity Framework.
