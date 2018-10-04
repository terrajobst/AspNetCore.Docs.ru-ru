---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity | Документация Майкрософт
author: HaoK
description: Этом учебнике показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS и электронной почты. Эта статья написана с Рик Андерсон ( @RickAndMSFT ), счетам...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b253923696e35e59c196578a232f53c11671d16
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577981"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity
====================
по [поздравить Хао](https://github.com/HaoK), [Пранавом Растоги](https://github.com/rustd), [Рик Андерсон]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Этом учебнике показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS и электронной почты.
> 
> Эта статья написана с Рик Андерсон ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Пранавом Растоги ([@rustd](https://twitter.com/rustd)), поздравить Хао и Suhas Joshi. NuGet образец был написан главным образом Хао поздравить.


В этом разделе объясняется следующее.

- [Построение образца удостоверений](#build)
- [Настройка SMS для двухфакторной проверки подлинности](#SMS)
- [Включение двухфакторной проверки подлинности](#enable2)
- [Как зарегистрировать поставщик двухфакторной проверки подлинности](#reg)
- [Объединить учетные записи социальных сетей и локального имени входа](#combine)
- [Блокировка учетной записи от атак методом подбора](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Построение образца удостоверений

В этом разделе описано вы используете NuGet, чтобы загрузить образец, который мы будем работать с. Начните с установки и запуска [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Установка Visual Studio [2013 с обновлением 2](https://go.microsoft.com/fwlink/?LinkId=390521) или более поздней версии.

> [!NOTE]
> Предупреждение: Необходимо установить Visual Studio [2013 с обновлением 2](https://go.microsoft.com/fwlink/?LinkId=390521) для работы с этим руководством.


1. Создайте новый ***пустой*** веб-проект ASP.NET.
2. В консоли диспетчера пакетов введите следующие команды:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   В этом руководстве мы будем использовать [SendGrid](http://sendgrid.com/) для отправки электронной почты и [Twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) для sms-сообщения. `Identity.Samples` Пакет устанавливает код, мы будем работать с.
3. Задайте [проект для использования SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Необязательный*: следуйте инструкциям в моей [руководства по электронной почте подтверждение](account-confirmation-and-password-recovery-with-aspnet-identity.md) для подключения SendGrid, а затем запустите приложение и зарегистрировать учетную запись электронной почты.
5. * Необязательно: * удалите демонстрации по электронной почте ссылку подтверждения код из примера ( `ViewBag.Link` кода в контроллере account. См. в разделе `DisplayEmail` и `ForgotPasswordConfirmation` методов и представлений razor действия).
6. <em>Необязательно: * удалите `ViewBag.Status` код из контроллеров учетной записи и управление ими, а также из *Views\Account\VerifyCode.cshtml</em> и <em>Views\Manage\VerifyPhoneNumber.cshtml</em> представлений razor. Кроме того, вы можете сохранить `ViewBag.Status` отображение, чтобы проверить, как работает это приложение локально без необходимости подключения и отправки электронной почты и SMS-сообщения.

> [!NOTE]
> Предупреждение: При изменении любого из параметров безопасности в этом образце, производства приложений потребуется пройти проверку подлинности, аудита безопасности, явным образом вызывает изменения, внесенные.


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
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Адрес:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Пространство имен:  
    `ASPSMSX2`
3. **Выяснение учетные данные поставщика SMS**  
  
   Twilio:  
   Из **панели мониторинга** вкладке учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.  
  
   ASPSMS:  
   Параметры учетной записи, перейдите к **Userkey** и скопируйте его вместе с вашей самоопределяющегося **пароль**.  
  
   В переменных позже мы сохраним эти значения `SMSAccountIdentification` и `SMSAccountPassword` .
4. **Указав идентификатор SenderID / инициатора**  
  
   Twilio:  
   Из **номера** вкладке, скопируйте свой номер телефона Twilio.  
  
   ASPSMS:  
   В рамках **разблокировать инициаторы** меню разблокировать один или несколько авторства или выберите инициатор буквенно-цифровых (не поддерживаются все сети).  
  
   Позже он будет храниться это значение в переменной `SMSAccountFrom` .
5. **Передача учетных данных поставщика SMS в приложение**  
  
   Сделать доступными для приложения учетные данные и номер телефона отправителя:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Безопасность — никогда не сохраняйте конфиденциальные данные в исходном коде. Запись и учетные данные добавляются в код выше для простоты в примере. См. в разделе Jon Atten [ASP.NET MVC: сохранить частные параметры Out из системы управления версиями](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Реализация передачи данных для поставщика SMS**  
  
   Настройка `SmsService` в класс *приложения\_Start\IdentityConfig.cs* файла.  
  
   В зависимости от используемых поставщика SMS активировать **Twilio** или **ASPSMS** раздел: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Запустите приложение и войдите с помощью учетной записи, которую вы ранее зарегистрировали.
8. Щелкните свой идентификатор пользователя, который активирует `Index` метода действия в `Manage` контроллера.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Нажмите кнопку Добавить.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Через несколько секунд вы получите текстовое сообщение с кодом проверки. Введите его и нажмите клавишу **отправить**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Представления управление показывает, что был добавлен ваш номер телефона.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Изучите код

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Метода действия в `Manage` контроллер задает сообщение о состоянии, в зависимости от предыдущего действия и приведены ссылки, чтобы добавить локальную учетную запись или изменить локальный пароль. `Index` Метод также отображает состояние или вашей 2FA телефонный номер, внешних имен входа, включил 2FA и помнить 2FA метод для этого браузера (описывается далее). Щелкните свой идентификатор пользователя (Электронная почта) в строке заголовка не передает сообщение. Щелкнув **номер телефона: удалить** связать передает `Message=RemovePhoneSuccess` как строку запроса.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Метод действия отображает диалоговое окно, введите номер телефона, который может получать SMS-сообщения.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Щелкнув **отправить проверочный код** кнопку учитывает номер телефона будет использоваться HTTP POST `AddPhoneNumber` метода действия.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Метод создает маркер безопасности, в которой будут устанавливаться в SMS-сообщения. Если служба SMS будет настроена, маркер отправляется в качестве строки &quot;ваш код безопасности &lt;маркера&gt;&quot;. `SmsService.SendAsync` Метод вызывается асинхронно, а затем перенаправляется приложение `VerifyPhoneNumber` метода действия (в котором отображаются следующее диалоговое окно), где можно ввести код проверки.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

После ввода кода и нажмите кнопку Отправить, код отправляется запрос HTTP POST `VerifyPhoneNumber` метода действия.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Метод проверяет переданные безопасности кода. Если код является правильным, номер телефона добавляется `PhoneNumber` поле `AspNetUsers` таблицы. Если этот вызов был успешным, `SignInAsync` вызывается метод:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Параметр задает ли сеанса проверки подлинности будет сохраняться в нескольких запросов.

При изменении профиля безопасности, создается и сохраняется в новую метку безопасности `SecurityStamp` поле *AspNetUsers* таблицы. Обратите внимание, что `SecurityStamp` поле отличается от безопасности cookie. Файл cookie безопасности не хранится в `AspNetUsers` таблицы (или любой другой в базе данных удостоверений). Маркер безопасности cookie подписывается самостоятельно с помощью [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) и создается с `UserId, SecurityStamp` и сведения о времени истечения срока действия.

По промежуточного слоя проверяет файл cookie при каждом запросе. `SecurityStampValidator` Метод в `Startup` класс обращений к базе данных и периодически проверяет метку безопасности, указанные в `validateInterval`. Это происходит каждые 30 минут (в нашем примере) можно только после изменения профиля безопасности. Чтобы свести к минимуму количество обращений к базе данных был выбран 30-минутный интервал.

`SignInAsync` Метод должен вызываться при внесении любых изменений в профиль безопасности. При изменении профиля безопасности, база данных находится обновлений `SecurityStamp` поле и без вызова `SignInAsync` будет находиться в системе метод *только* до следующего выполнения конвейера OWIN обращается к базе данных ( `validateInterval`). Это можно проверить, изменив `SignInAsync` метод для немедленный возврат, а также установка файла cookie `validateInterval` свойство от 30 минут до 5 секунд:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Выше изменения кода, можно изменить профиль безопасности (например, при изменении состояния **включено два фактора**) и вы выйдете из в течение 5 секунд при `SecurityStampValidator.OnValidateIdentity` метод завершается ошибкой. Удалить строки, возврата в `SignInAsync` метод, сделать другой профиль безопасности изменить, и вы не выйдете. `SignInAsync` Метод создает новый файл cookie безопасности.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Включение двухфакторной проверки подлинности

В примере приложения необходимо включить двухфакторную проверку подлинности (2FA) с помощью пользовательского интерфейса. Чтобы включить 2FA, щелкните свой идентификатор пользователя (псевдоним электронной почты) на панели навигации.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Щелкните Включить 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Выйдите, а затем войдите снова. Если вы включили функцию по электронной почте (см. в разделе my [предыдущем учебном курсе](account-confirmation-and-password-recovery-with-aspnet-identity.md)), можно выбрать SMS или электронной почты для 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Проверьте кодовая страница отображается, где можно ввести код (от SMS или электронной почты).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Щелкнув **запомнить браузер** флажок будет исключить от необходимости использовать 2FA войти в систему с этого компьютера и браузера. Включение 2FA и щелкнув **запомнить браузер** предоставит вам строгого 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, до тех пор, пока они не имеют доступа к компьютеру. Это можно сделать на любой закрытый компьютер, который регулярно использовать. Установив **запомнить браузер**, вы получаете дополнительную защиту 2FA с компьютеров, вы не используете регулярно, и вы получаете удобства на не нужно изучать через 2FA на собственных компьютерах. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Как зарегистрировать поставщик двухфакторной проверки подлинности

При создании нового проекта MVC, *IdentityConfig.cs* файл содержит следующий код, чтобы зарегистрировать поставщик двухфакторной проверки подлинности:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Добавить номер телефона для 2FA

`AddPhoneNumber` Метода действия в `Manage` контроллер создает маркер безопасности, и отправляет его на телефоне номер, который вы предоставили.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

После отправки маркера, он перенаправляет `VerifyPhoneNumber` метода действия, где можно ввести код для регистрации SMS 2FA. SMS 2FA не используется, пока вы подтвердили номер телефона.

## <a name="enabling-2fa"></a>Включение 2FA

`EnableTFA` 2FA включает метод действия:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Примечание `SignInAsync` должен вызываться так, как включить 2FA является изменение профиля безопасности. При включении 2FA, пользователю могут потребоваться 2FA для ведения журнала, с помощью подхода 2FA, зарегистрированные ими в (SMS и электронной почты в образце).

Можно добавить дополнительные поставщики 2FA, такие как генераторы QR-кода или можно написать вы являетесь владельцем (см. в разделе [с помощью Google Authenticator с ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Коды 2FA создаются с помощью [времени одноразовый пароль алгоритм на основе](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) и коды действительны в течение шести минут. Если более чем шесть минут, чтобы ввести код, вы получите сообщение об ошибке Недопустимый код.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Объединить учетные записи социальных сетей и локального имени входа

Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности &quot; RickAndMSFT@gmail.com &quot; сначала создается как локальное имя входа, но можно создать учетную запись как журнал социальных сетей в первой, а затем добавить локальное имя входа.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Щелкните **управление** ссылку. Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Щелкните ссылку, чтобы другой журнал в службе и принимать запросы приложений. Были объединены две учетные записи, вы сможете выполнить вход с использованием либо учетной записи. Вы можете добавить локальные учетные записи, в случае их социальных сетей журнала в службе проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.

На следующем рисунке, Tom является журналом социальных сетей в (который также можно увидеть из **внешних имен входа: 1** на странице).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Щелкнув **выберите пароль** позволяет добавлять в локальный журнал на связанные с той же учетной записи.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Блокировка учетной записи от атак методом подбора

Учетные записи можно защитить приложения от атак перебором по словарю путем включения блокировки пользователя. В следующем коде в `ApplicationUserManager Create` метод настраивает блокировке:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Приведенный выше код позволяет блокировки для двухфакторной проверки подлинности только. При блокировке для имен входа можно включить, изменив `shouldLockout` в значение true в `Login` метода контроллера учетных записей, мы рекомендуем не включать блокировке для имен входа, поскольку это делает уязвимо для учетной записи [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) входа атаки. В образце кода, блокировка отключена учетная запись администратора, созданной в `ApplicationDbInitializer Seed` метод:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Пользователю нужно было иметь учетную запись электронной почты проверенные

Следующий код требует, чтобы учетная запись электронной почты проверенные сможет войти пользователь:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Как SignInManager проверяет наличие 2FA требование

Локальный вход и социальных сетей входа проверьте, включена ли 2FA. Если включен 2FA, `SignInManager` возвращает входа `SignInStatus.RequiresVerification`, и пользователь будет перенаправлен к `SendCode` метода действия, где они принесут ввести код для выполнения в журнал в последовательности. Если пользователь имеет RememberMe задается в файле cookie локальных пользователей, `SignInManager` вернет `SignInStatus.Success` и не будет проходить через 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

В следующем коде показан `SendCode` метода действия. Объект [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) создается со всеми методами 2FA, доступны пользователю. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) передается [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) вспомогательного приложения, который позволяет пользователю выбрать подход 2FA (обычно по электронной почте и SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Когда пользователь отправляет подход 2FA, `HTTP POST SendCode` вызывается метод действия, `SignInManager` отправляет код 2FA и пользователь перенаправляется на `VerifyCode` метода действия, где они могут ввести код, чтобы завершить вход.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA блокировки

Несмотря на то, что при сбое попытки пароль имени входа можно задать блокировку учетных записей, такой подход делает имя входа подвержена [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) блокировки. Мы рекомендуем использовать только с 2FA блокировки учетных записей. При `ApplicationUserManager` будет создан, пример кода задает 2FA блокировки и `MaxFailedAccessAttemptsBeforeLockout` пять. После того как пользователь вошел в систему (с помощью локальной учетной записи или учетной записи социальной сети), каждая неудачная попытка 2FA хранится и если достигается максимальное число попыток, пользователь будет заблокирован на пять минут (можно задать блокировке времени с помощью `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Рекомендуемые ресурсы по ASP.NET Identity](../getting-started/aspnet-identity-recommended-resources.md) полный список идентификаторов блоги, видеоролики, учебники и great поэтому ссылки.
- [Приложение MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) также показано, как добавить данные профиля в таблице users.
- [ASP.NET MVC и удостоверений 2.0: представление об основах](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) , Джон Atten.
- [Подтверждение учетной записи и восстановление пароля в ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Введение в ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Объявление о выпуске RTM удостоверения ASP.NET 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) с Пранавом Растоги.
- [Удостоверение ASP.NET 2.0: Настройки учетной записи проверки авторизации двухфакторной](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) , Джон Atten.
