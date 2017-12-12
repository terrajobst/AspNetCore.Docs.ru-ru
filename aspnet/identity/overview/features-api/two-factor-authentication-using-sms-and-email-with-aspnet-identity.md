---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity | Документы Microsoft"
author: HaoK
description: "Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS и адрес электронной почты. В этой статье было написано с Рик Андерсон ( @RickAndMSFT ), счетам..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity
====================
по [поздравить Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS и адрес электронной почты.
> 
> В этой статье было написано с Рик Андерсон ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), поздравить Hao и Suhas Joshi. NuGet образец был написан главным образом Hao поздравить.


В этом разделе объясняется следующее.

- [Построение образца удостоверений](#build)
- [Настройка SMS для двухфакторной проверки подлинности](#SMS)
- [Включение двухфакторной проверки подлинности](#enable2)
- [Как зарегистрировать поставщик двухфакторной проверки подлинности](#reg)
- [Объединение социальных сетей и локальных учетных записей](#combine)
- [Блокировка учетной записи, от атак методом подбора](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Построение образца удостоверений

В этом разделе будет использоваться NuGet для загрузки образца, который мы работаем над. Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка Visual Studio [2013 обновление 2](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо установить Visual Studio [2013 обновление 2](https://go.microsoft.com/fwlink/?LinkId=390521) для завершения этого учебника.


1. Создайте новый ***пустой*** веб-проекте ASP.NET.
2. В консоли диспетчера пакетов введите следующие команды:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 В этом учебнике мы будем использовать [SendGrid](http://sendgrid.com/) для отправки электронной почты и [Twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) для sms-сообщения. `Identity.Samples` Пакет устанавливает код, мы будем работать с.
3. Задать [проекта для использования SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Необязательный*: следуйте инструкциям в моей [учебник по электронной почте подтверждение](account-confirmation-and-password-recovery-with-aspnet-identity.md) подключить SendGrid и запустить приложение и зарегистрировать учетную запись электронной почты.
5. * Необязательно: * удалите Демонстрация по электронной почте ссылку подтверждения код из примера ( `ViewBag.Link` кода в контроллера учетных записей. В разделе `DisplayEmail` и `ForgotPasswordConfirmation` методы действий и представления razor).
6. * Необязательно: * удалите `ViewBag.Status` код из контроллеров учетной записи и управление ими и *Views\Account\VerifyCode.cshtml* и *Views\Manage\VerifyPhoneNumber.cshtml* представлений razor. Кроме того, можно сохранить `ViewBag.Status` отображение, чтобы проверить, как работает это приложение локально, без необходимости подключать и отправки электронной почты и SMS-сообщений.

> [!NOTE]
> Предупреждение: При изменении параметров безопасности в этом образце производства приложения потребуется проходить аудит безопасности, которая явно вызывает изменения.


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
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 Адрес:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Пространство имен:  
    `ASPSMSX2`
3. **Изучив учетные данные пользователя поставщика SMS**  
  
 Twilio:  
 Из **мониторинга** вкладка учетной записи Twilio, Копировать **ИД безопасности учетной записи** и **маркер проверки подлинности**.  
  
 ASPSMS:  
 Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с самостоятельно определенных **пароль**.  
  
 Мы позже хранит эти значения в переменных `SMSAccountIdentification` и `SMSAccountPassword` .
4. **Указав идентификатор отправителя / инициатора**  
  
 Twilio:  
 Из **номера** вкладки, скопируйте Twilio номер телефона.  
  
 ASPSMS:  
 В пределах **разблокировать инициаторы** меню разблокирования одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети).  
  
 Это значение будет позже хранятся в переменной `SMSAccountFrom` .
5. **Передача учетных данных поставщика SMS в приложение**  
  
 Предоставьте доступ к приложению учетные данные и номер телефона отправителя.

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Безопасность — никогда не конфиденциальных данных в хранилище в исходном коде. Учетная запись и учетные данные добавляются выше, чтобы не усложнять в образце кода. . В разделе Jon Atten [ASP.NET MVC: сохранить частные параметры из системы управления версиями](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Реализация передачи данных для поставщика SMS**  
  
 Настройка `SmsService` класса в *приложения\_Start\IdentityConfig.cs* файла.  
  
 В зависимости от используемых поставщика SMS активировать **Twilio** или **ASPSMS** раздела: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Запустите приложение и войти с использованием учетной записи, которую вы ранее зарегистрированного.
8. Щелкните идентификатор пользователя, который активирует `Index` метода действия в `Manage` контроллера.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Нажмите кнопку Добавить.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Через несколько секунд вы получите SMS с кодом проверки. Введите его и нажмите клавишу **отправить**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Представления управление показывает, что был добавлен в ваш номер телефона.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Анализ кода

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Метода действия в `Manage` задает сообщение о состоянии в зависимости от предыдущего действия контроллера, а также ссылки на добавить локальную учетную запись или изменить локальный пароль. `Index` Метод также отображает состояние или вашей 2FA телефонный номер, внешних имен входа, 2FA включен и запоминать, метод 2FA для этого браузера (рассматривается позже). Щелкнув код пользователя (электронной почты) в строке заголовка не передает сообщение. Щелкнув **номер телефона: удалите** связать передает `Message=RemovePhoneSuccess` как строка запроса.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Метод действия отображает диалоговое окно, введите номер телефона, который может получить SMS-сообщений.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Щелкнув **отправить код проверки** кнопка инициирует передачу номер телефона для HTTP POST `AddPhoneNumber` метода действия.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Метод создает маркер безопасности, в которой будут устанавливаться в SMS-сообщения. Если служба SMS была настроена, маркер отправляется в качестве строки &quot;ваш код безопасности &lt;маркера&gt;&quot;. `SmsService.SendAsync` Метод вызывается асинхронно, а затем перенаправляется приложение `VerifyPhoneNumber` метода действия (в котором отображается следующее диалоговое окно), где можно ввести код проверки.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Введите код и нажмите кнопку Отправить, HTTP POST отправляется код `VerifyPhoneNumber` метода действия.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Метод проверяет, отправленное защитный код. Если правильности кода, номер телефона добавляется `PhoneNumber` поле `AspNetUsers` таблицы. Если этот вызов был успешным, `SignInAsync` вызывается метод:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Параметр задает ли сеанс проверки подлинности сохраняется в нескольких запросах.

При изменении профиля безопасности новой метки безопасности создается и сохраняется в `SecurityStamp` поле *AspNetUsers* таблицы. Следует отметить, что `SecurityStamp` поля отличается от безопасности cookie. Файл cookie безопасности не хранятся в `AspNetUsers` таблицы (или любом другом месте в базе данных удостоверений). Маркер безопасности cookie самостоятельно подписывается с помощью [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) и создается `UserId, SecurityStamp` и сведения о времени истечения срока действия.

По промежуточного слоя файлов cookie проверяет куки-файл для каждого запроса. `SecurityStampValidator` Метод в `Startup` класс обращений к базе данных и периодически проверяет метку безопасности в соответствии с `validateInterval`. Это происходит каждые 30 минут (в нашем примере) только изменения профиля безопасности. Чтобы свести к минимуму обращений к базе данных был выбран интервал 30 минут.

`SignInAsync` Метод должен вызываться при любом изменении профиля безопасности. При изменении профиля безопасности, база данных находится в обновлений `SecurityStamp` поле и без вызова `SignInAsync` будет находиться в системе метод *только* до следующего запуска конвейер OWIN обращается к базе данных ( `validateInterval`). Можно проверить это, изменив `SignInAsync` метод для немедленный возврат, а также установка файла cookie `validateInterval` свойство за 30 минут до 5 секунд:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Выше изменений кода, можно изменить профиль безопасности (например, при изменении состояния **включены два фактора**) и будет выхода в течение 5 секунд при `SecurityStampValidator.OnValidateIdentity` метод завершается ошибкой. Удалить строки, возврата в `SignInAsync` метода, сделать другой профиль безопасности изменить, и вам будет не вышел из. `SignInAsync` Метод создает новый файл cookie безопасности.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Включение двухфакторной проверки подлинности

В примере приложения необходимо включить двухфакторную проверку подлинности (2FA) с помощью пользовательского интерфейса. Чтобы включить 2FA, щелкните свой идентификатор пользователя (псевдоним электронной почты) на панели навигации.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Щелкните Включить 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Выйдите из системы, а затем войдите снова. Если вы включили функцию электронной почты (см. Мои [с предыдущим учебником](account-confirmation-and-password-recovery-with-aspnet-identity.md)), можно выбрать SMS или по электронной почте для 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Проверьте кодовая страница отображается, где можно ввести код (от SMS или по электронной почте).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA войти в систему с этого компьютера и обозреватель. Включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают надежный 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, пока они не имеют доступа к компьютеру. Это можно сделать на любом компьютере закрытый, которые регулярно использовать. Установив **Запомнить этот браузер**, получить дополнительную защиту 2FA на компьютерах, не пользуетесь регулярно и получения удобства на отсутствие необходимости проходить через 2FA на собственных компьютерах. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Как зарегистрировать поставщик двухфакторной проверки подлинности

При создании нового проекта MVC *IdentityConfig.cs* файл содержит следующий код, чтобы зарегистрировать поставщика двухфакторной проверки подлинности:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Добавить номер телефона для 2FA

`AddPhoneNumber` Метода действия в `Manage` контроллера создает маркер безопасности и отправляет его на телефоне число, вы предоставили.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

После отправки маркер, он перенаправляет `VerifyPhoneNumber` метод действия, где можно ввести код для регистрации 2FA SMS. SMS 2FA не используется, пока вы проверили номер телефона.

## <a name="enabling-2fa"></a>Включение 2FA

`EnableTFA` Метод действия включает 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Примечание `SignInAsync` должен вызываться так, как включить 2FA изменение профиля безопасности. При включении 2FA пользователю могут потребоваться 2FA для ведения журнала, способами 2FA регистрации (SMS и электронной почты в образце).

Можно добавить дополнительные 2FA поставщиков, например генераторы QR-кода или можно написать вы являетесь владельцем (см. [с помощью Google Authenticator с ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Коды 2FA создаются с помощью [время одноразовый пароль алгоритм на основе](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) и коды действительны в течение шести минут. Если более шести минут, введите код, вы получите сообщение об ошибке Недопустимый код.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Объединение социальных сетей и локальных учетных записей

Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности &quot; RickAndMSFT@gmail.com &quot; сначала создается как локальное имя входа, но можно создать учетную запись в качестве социальных журналов в первой, а затем добавить локальный вход.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Щелкните **управление** ссылку. Примечание внешних 0 (социальных имена входа), связанные с этой учетной записи.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Щелкните ссылку, чтобы другой журнал в службе, которая принимает запросы приложения. Были объединены две учетные записи, вы сможете войти в систему с любой учетной записи. Может потребоваться пользователям добавлять локальные учетные записи, в случае их социальных вход службы проверки подлинности не работает, или что более вероятно, они утрачен доступ к учетной записи социальных сетей.

На следующем рисунке Tom является социальных вход (что можно узнать из **внешних имен входа: 1** отображаться на странице).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Щелкнув **подбора пароля** можно добавить в локальный журнал на связанные с той же учетной записи.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Блокировка учетной записи, от атак методом подбора

Учетные записи можно защитить приложения от атак методом подбора путем включения блокировки пользователя. В следующем примере кода в `ApplicationUserManager Create` метод настраивает блокировка:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Приведенный выше код позволяет блокировки для двухфакторной проверки подлинности только. Хотя блокировка для имен входа, можно включить, изменив `shouldLockout` значение true в `Login` метод контроллера учетных записей, мы рекомендуем не включена блокировка для имен входа, потому что позволяет учетной записи подвержены [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) входа атаки. В образце кода, блокировка отключена для учетной записи администратора, созданной в `ApplicationDbInitializer Seed` метод:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Требуется учетная запись электронной почты проверенного пользователя

Следующий код требует, чтобы пользователь имел проверенные электронной почты учетной записи для входа:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Как SignInManager проверяет для 2FA требования

Локальный вход и проверьте, включена ли 2FA социальных вход. При включении 2FA `SignInManager` входа возвращает `SignInStatus.RequiresVerification`, и пользователь будет перенаправлен на `SendCode` метод действия, где их придется вводить код для завершения журнала в последовательности. Если у пользователя есть RememberMe задается в файле cookie локальных пользователей `SignInManager` вернет `SignInStatus.Success` и им не нужно проходить через 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

В следующем коде показано `SendCode` метода действия. Объект [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) создается со всеми методами 2FA, разрешенных для текущего пользователя. [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) передается [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) вспомогательного приложения, который позволяет пользователю выбрать подход 2FA (обычно электронной почты и SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Когда пользователь отправляет подход 2FA `HTTP POST SendCode` вызывается метод действия, `SignInManager` отправляет 2FA кода и пользователь перенаправляется на `VerifyCode` метод действия, где можно ввести код, чтобы завершить вход.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA блокировки

Несмотря на то, что при сбое попытки пароль имени входа можно задать блокировки учетной записи, такой подход делает ваше имя входа подвержен атакам с [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) блокировки. Мы рекомендуем использовать только с 2FA блокировки учетной записи. При `ApplicationUserManager` будет создан, образец кода задает 2FA блокировки и `MaxFailedAccessAttemptsBeforeLockout` до 5. После пользователь вошел в систему (с помощью локальной учетной записи или учетной записи социальных сетей), каждая неудачная попытка 2FA хранится и при достижении максимального попыток пользователь заблокирован на пять минут (можно задать блокировка времени с `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [ASP.NET Identity, рекомендуется использовать ресурсы](../getting-started/aspnet-identity-recommended-resources.md) полный список идентификаторов блоги, видеоролики, учебники и высококачественных таким ОБРАЗОМ ссылки.
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и входа в Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) также показано, как добавить данные профиля в таблицу пользователей.
- [ASP.NET MVC и удостоверение 2.0: представление об основах](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) , Джон Atten.
- [Подтверждение учетной записи и пароль восстановления в ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Введение в ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Объявляет о RTM ASP.NET удостоверения 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) по Pranav Rastogi.
- [Удостоверение ASP.NET 2.0: Настройки учетной записи проверки авторизации двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) , Джон Atten.
