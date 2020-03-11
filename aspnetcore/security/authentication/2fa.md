---
title: Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить двухфакторную проверку подлинности (2FA) с помощью приложения ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652726"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core

[Рик Андерсон (](https://twitter.com/RickAndMSFT) и [Швейцарский разработчики](https://github.com/Swiss-Devs)

>[!WARNING]
> Два фактора проверки подлинности (2FA) проверки подлинности приложения с помощью по времени одноразовый пароль алгоритм (TOTP), являются отрасли, рекомендуемый подход для 2FA. 2FA с помощью TOTP предпочтительнее SMS 2FA. Дополнительные сведения см. [в статье Включение создания QR-кода для приложений TOTP Authenticator в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) для ASP.NET Core 2,0 и более поздних версий.

Этом руководстве показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS. Инструкции предоставляются для [twilio](https://www.twilio.com/) и [аспсмс](https://www.aspsms.com/asp.net/identity/core/testcredits/), но можно использовать любой другой поставщик SMS. Перед началом работы с этим руководством мы рекомендуем завершить [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm) .

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Как скачать](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Создание проекта ASP.NET Core

Создайте новое ASP.NET Core веб-приложение с именем `Web2FA` с учетными записями отдельных пользователей. Следуйте инструкциям в <xref:security/enforcing-ssl>, чтобы настроить и требовать протокол HTTPS.

### <a name="create-an-sms-account"></a>Создание учетной записи SMS

Создайте учетную запись SMS, например из [twilio](https://www.twilio.com/) или [аспсмс](https://www.aspsms.com/asp.net/identity/core/testcredits/). Запишите учетные данные проверки подлинности (для twilio: accountSid и authToken для ASPSMS: Userkey и пароль).

#### <a name="figuring-out-sms-provider-credentials"></a>Выяснение учетные данные поставщика SMS

**Twilio**

На вкладке Панель мониторинга учетной записи Twilio скопируйте **SID учетной записи** и **токен проверки подлинности**.

**АСПСМС:**

В параметрах учетной записи перейдите по адресу **Userkey** и скопируйте его вместе с **паролем**.

Позже эти значения будут храниться в с помощью средства Secret-Manager в ключах `SMSAccountIdentification` и `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Указав идентификатор SenderID / инициатора

**Twilio:** На вкладке числа скопируйте **номер телефона**Twilio.

**Аспсмс:** В меню Источник разблокировки Разблокируйте один или несколько источник или выберите буквенно-цифровой инициатор (не поддерживается всеми сетями).

Позже это значение будет храниться в средстве Secret-Manager в рамках ключа `SMSAccountFrom`.

### <a name="provide-credentials-for-the-sms-service"></a>Укажите учетные данные для службы SMS

Мы будем использовать [шаблон параметров](xref:fundamentals/configuration/options) для доступа к учетной записи пользователя и параметрам ключа.

* Создание класса для извлечения ключа безопасности SMS. В этом примере класс `SMSoptions` создается в файле *Services/смсоптионс. CS* .

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Задайте `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с помощью [средства Secret-Manager](xref:security/app-secrets). Пример:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* Добавьте пакет NuGet для поставщика SMS. Из консоли диспетчера пакетов (PMC) выполните:

**Twilio**

`Install-Package Twilio`

**АСПСМС:**

`Install-Package ASPSMS`

* Добавьте код в файл *Services/мессажесервицес. CS* , чтобы включить SMS. Использование Twilio или разделе ASPSMS:

**Twilio**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**АСПСМС:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Настройка запуска для использования `SMSoptions`

Добавьте `SMSoptions` в контейнер службы в методе `ConfigureServices` в *Startup.CS*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Включение двухфакторной проверки подлинности

Откройте файл представления Razor views */Manage/index. cshtml* и удалите символы комментария (поэтому разметка не записывается в комментарий).

## <a name="log-in-with-two-factor-authentication"></a>Войдите с помощью двухфакторной проверки подлинности

* Запустите приложение и зарегистрируйте нового пользователя

![Веб-приложение Register, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* Коснитесь имени пользователя, которое активирует метод действия `Index` в окне Управление контроллером. Затем коснитесь ссылки **Добавить** номер телефона.

![Управление представления – нажать ссылку «Добавить»](2fa/_static/login2fa2.png)

* Добавьте номер телефона, который будет получать код проверки, и нажмите кнопку **Отправить проверочный код**.

![Добавить страницу номер телефона](2fa/_static/login2fa3.png)

* Вы получите текстовое сообщение с кодом проверки. Введите его и нажмите кнопку **Отправить** .

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

Если вы не получили текстовое сообщение, см. в разделе страницы журнала twilio.

* Представления управление показывает, что ваш номер телефона успешно добавлен.

![Управление представление — номер телефона успешно добавлен](2fa/_static/login2fa5.png)

* Коснитесь кнопки **включить** , чтобы включить двухфакторную проверку подлинности.

![Управления представлением — Включение двухфакторной проверки подлинности](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Тестирование двухфакторной проверки подлинности

* Выйдите из системы.

* Войдите в систему.

* Учетной записи пользователя включена двухфакторная проверка подлинности, поэтому вы должны предоставить второй фактор проверки подлинности. В этом руководстве вы включили Проверка номера телефона. Встроенные шаблоны позволяют настроить почту в качестве второго фактора. Можно настроить дополнительные факторы проверки подлинности, например QR-коды, второй. Нажмите кнопку **Отправить**.

![Отправить код проверки представления](2fa/_static/login2fa7.png)

* Введите код, полученный в SMS-сообщения.

* Если установить флажок **Запомнить этот браузер** , вам не нужно будет использовать 2FA для входа в систему при использовании того же устройства и браузера. Включение 2FA и нажатие кнопки " **Запомнить" Этот браузер** обеспечит надежную защиту 2FA от злоумышленников, пытающихся получить доступ к вашей учетной записи, если у них нет доступа к вашему устройству. Это можно сделать на любом устройстве закрытого, которые регулярно использовать. Установив флажок **Запомнить этот браузер**, вы получите дополнительную защиту 2FA от устройств, которые вы не используете регулярно, и вы получите удобный способ, чтобы не выполнять переход на 2FA на своих устройствах.

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Блокировка учетной записи, обеспечивающие защиту от атак методом подбора

Блокировка учетной записи рекомендуется с 2FA. Когда пользователь войдет в приложение через локальную учетную запись или учетную запись социальной сети, каждая неудачная попытка 2FA хранится. После достижения максимального неудачных попыток доступа пользователя блокируется (по умолчанию: 5-минутного блокировки после 5 неудачных попыток доступа). Сбрасывает счетчик попыток неудачные попытки доступа успешной проверки подлинности и сбрасывает часы. Максимальное количество неудачных попыток доступа и время блокировки можно задать с помощью [максфаиледакцессаттемптс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) и [дефаултлоккауттимеспан](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Следующие настраивает блокировки учетных записей для 10 минут после десяти неудачных попыток доступа:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Убедитесь, что [пассвордсигнинасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) задает `lockoutOnFailure` `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
