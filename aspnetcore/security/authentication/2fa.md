---
title: Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core
author: rick-anderson
description: Узнайте, как настроить двухфакторную проверку подлинности (2FA) с приложением ASP.NET Core.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Двухфакторная проверка подлинности с помощью SMS в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [разработчики швейцарский](https://github.com/Swiss-Devs)

В разделе [Создание включить QR-код для приложения для проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) Core ASP.NET 2.0 и более поздних версий.

Этого учебника показано, как настроить двухфакторную проверку подлинности (2FA) с помощью SMS. Инструкции, приведенные для [twilio](https://www.twilio.com/) и [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), но можно использовать любой другой поставщик SMS. Рекомендуется [подтверждение учетной записи и пароль восстановления](xref:security/authentication/accconfirm) перед запуском этого учебника.

Представление [полного примера](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Загрузка](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Создайте новый проект ASP.NET Core

Создать новый веб-приложение ASP.NET Core с именем `Web2FA` для каждой учетной записи. Следуйте инструкциям в [применять SSL в приложении ASP.NET Core](xref:security/enforcing-ssl) для настройки и требовать SSL.

### <a name="create-an-sms-account"></a>Создание учетной записи SMS

Создание учетной записи SMS, например, из [twilio](https://www.twilio.com/) или [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Запишите учетные данные проверки подлинности (для twilio: accountSid и authToken для ASPSMS: использованием ключа пользователя и пароль).

#### <a name="figuring-out-sms-provider-credentials"></a>Изучив учетные данные поставщика SMS

**Twilio:**  
На вкладке панели мониторинга учетной записи Twilio, скопируйте **ИД безопасности учетной записи** и **маркер проверки подлинности**.

**ASPSMS:**  
Параметры учетной записи, перейдите к **использованием ключа пользователя** и скопируйте его вместе с вашей **пароль**.

Позже он будет храниться эти значения с помощью секрет диспетчера в ключи `SMSAccountIdentification` и `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Указав идентификатор отправителя / инициатора

**Twilio:**  
На вкладке номера скопируйте вашей Twilio **номер телефона**. 

**ASPSMS:**  
В меню разблокирования инициаторы разблокировать одного или нескольких источников или выберите инициатор буквенно-цифровых (поддерживаются не все сети). 

Это значение с помощью средства диспетчера секрет раздел позже будет храниться `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Укажите учетные данные для службы SMS

Мы будем использовать [параметры шаблона](xref:fundamentals/configuration/options) для доступа к параметрам учетной записи и ключа пользователя. 

   * Создание класса для выборки защищенный ключ SMS. Для этого образца `SMSoptions` класса создается в *Services/SMSoptions.cs* файла.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Задать `SMSAccountIdentification`, `SMSAccountPassword` и `SMSAccountFrom` с [секрет диспетчера](xref:security/app-secrets). Пример:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Добавьте пакет NuGet для поставщика SMS. Из пакета Диспетчер консоли (PMC) запуска:

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Добавьте код в *Services/MessageServices.cs* файл, чтобы включить SMS. Используйте Twilio или разделе ASPSMS:


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Настройка запуска для использования `SMSoptions`

Добавить `SMSoptions` в контейнер службы в `ConfigureServices` метод в *файла Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Включение двухфакторной проверки подлинности

Откройте *Views/Manage/Index.cshtml* файл представления Razor и удалите символы комментариев (поэтому разметка не закомментированного).

## <a name="log-in-with-two-factor-authentication"></a>Войдите с помощью двухфакторной проверки подлинности

* Запустите приложение и регистрации нового пользователя

![Веб-приложение регистра, откройте представление в Microsoft Edge](2fa/_static/login2fa1.png)

* Нажмите на имя пользователя, который активирует `Index` метод действия в контроллере управление. Выберите номер телефона **добавить** ссылку.

![Управление представления](2fa/_static/login2fa2.png)

* Добавить номер телефона, получать код проверки и коснитесь **отправить код проверки**.

![Добавление номера страницы](2fa/_static/login2fa3.png)

* Вы получите SMS с кодом проверки. Введите его и нажмите кнопку **отправки**

![Проверить номер телефона страницу](2fa/_static/login2fa4.png)

Если не получен текстовое сообщение, посетите страницу twilio журнала.

* Представления управление показывает, что ваш номер телефона успешно добавлен.

![Управление представления](2fa/_static/login2fa5.png)

* Коснитесь **включить** Включение двухфакторной проверки подлинности.

![Управление представления](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Тестирование двухфакторной проверки подлинности

* Выйдите из системы.

* Вход.

* Учетная запись пользователя включена двухфакторной проверки подлинности, поэтому вы должны предоставить второй фактор проверки подлинности. В этом учебнике вы включили Проверка телефона. Встроенные шаблоны также можно настроить электронную почту в качестве второго фактора. Можно настроить дополнительные факторы проверки подлинности, такие как QR-коды, второй. Коснитесь **отправить**.

![Отправить в представлении кода проверки](2fa/_static/login2fa7.png)

* Введите код, который вы получаете в SMS-сообщения.

* Щелкнув **Запомнить этот браузер** флажок Исключить вас от необходимости использовать 2FA вход при использовании одного устройства и браузера. Включение 2FA и щелкнув **Запомнить этот браузер** обеспечивают надежный 2FA защиты от злоумышленников, пытается получить доступ к вашей учетной записи, пока они не имеют доступа к устройству. Это можно сделать на любом устройстве закрытого регулярно используемых. Установив **Запомнить этот браузер**, получить дополнительную защиту 2FA с устройств, которые вы не используете регулярно и получения удобства на отсутствие необходимости проходить через 2FA на собственных устройствах.

![Проверить представление](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Блокировки учетных записей для защиты от атак методом подбора

С 2FA рекомендуется блокировки учетной записи. Когда пользователь входит через локальной учетной записи или учетной записи социальных сетей, каждого Неудачная попытка 2FA сохраняется. При достижении максимального неудачных попыток доступа пользователя блокируется (по умолчанию: 5-минутного блокировки после 5 неудачных попыток доступа). Сбрасывает счетчик неудачных доступов попыток прошли проверку подлинности и сбрасывает часы. Максимально неудачных попыток доступа и время блокировки может быть задано с [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) и [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Следующие настраивает блокировки учетных записей на 10 минут после 10 неудачных попыток доступа:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Убедитесь, что [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) задает `lockoutOnFailure` для `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
